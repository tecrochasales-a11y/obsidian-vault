# Workflows — Fluxos de Trabalho

## Fluxo: Lead entra pelo Painel do Corretor

```
Painel do Corretor (Trindade)
    ↓ (GraphQL API)
Chatwoot Kanban Sync (cron */5)
    ↓ (chatwoot_painel_map)
Chatwoot (conversa criada)
    ↓ (n8n automation)
WhatsApp (mensagem para corretor)
```

### Detalhes
1. Lead criado no Painel do Corretor
2. Script `/var/www/chatwoot-kanban/run-sync.sh` executa a cada 5 min
3. Mapeia phone → Chatwoot conversation_id na tabela `chatwoot_painel_map`
4. n8n dispara mensagem WhatsApp para corretor responsável

---

## Fluxo: Corretor clica "Link para Corretor" (DocCorretor)

```
Admin clica "Link para Corretor"
    ↓
Token gerado + pasta criada (client_id=null)
    ↓
Link copiado com folder_id
    ↓
Externo recebe link, acessa formulário
    ↓
Externo preenche (nome, email, documento)
    ↓
API atualiza pasta com client_id
    ↓
Documento fica disponível no DocCorretor
```

### ⚠️ NÃO ALTERAR
Este fluxo é crítico. A pasta é criada ANTES do externo preencher. Sem este fluxo, o DocCorretor não funciona.

---

## Fluxo: Alerta de Reunião Atrasada (ComercialRS)

```
Cron */5 task_alerts.py
    ↓
Query: tasks WHERE status='todo' AND dueDate < NOW()
    ↓
Verifica throttle (alert_throttle table)
    ↓ (se não bloqueado)
Envia WhatsApp para responsible_id
    ↓
Registra em alert_throttle (expira em 2h30)
```

### Anti-spam
- Lock 9000s (2h30) por responsável
- `message_log` check para evitar duplicados

---

## Fluxo: WhatsApp Button Click (ComercialRS)

```
Cliente clica botão WhatsApp
    ↓
Meta WhatsApp API → POST /webhook (porta 8083)
    ↓
meta_webhook_vps_service.py processa
    ↓
Identifica task via button payload
    ↓
Atualiza task status no Supabase
    ↓
Responde ao cliente
```

### ⚠️ CUIDADO
- Webhook configurado no Meta Developer Portal
- Não mexer no banco quando webhook já está no site UAZAPI
- Alterar = risco de bloqueio do número

---

## Fluxo: Deploy ComercialRS (VPS)

```
Desenvolvedor faz изменения no código
    ↓
Commit + push para GitHub
    ↓ (opcional)
VPS recebe via webhook ou pull manual
    ↓
Build: nvm use 20 && npm run build
    ↓
dist/ copiado para /var/www/comercialrs/dist/
    ↓
Nginx serve novo build
```

### Build rápido
```bash
cd /var/www/comercialrs
nvm use 20
npm run build
```

---

## Fluxo: Deploy DocCorretor (VPS)

```
Build local ou no VPS
    ↓
PATH="/root/.nvm/versions/node/v20.20.2/bin:$PATH" nvm use 20
node ./node_modules/vite/bin/vite.js build
    ↓
dist/ → /var/www/documentos2/app/dist/
    ↓
sudo systemctl restart documentos
```

### Verificar
```bash
systemctl status documentos
curl -s https://documentos.rochasalesseguros.com.br | head -5
```

---

## Fluxo: Migrar documento do Google Drive (DocCorretor)

```
Identificar documento no Google Drive
    ↓
Fazer download via gdown (url pública)
    ↓
Upload para Supabase Storage
    ↓
Criar registro em documents + storage_files
    ↓
Atualizar folder com client_id
    ↓
Verificar permissões (o+r)
```

### Comando típico
```bash
gdown "https://drive.google.com/uc?id=FILE_ID" -O /tmp/doc.pdf
# Upload via API ou script
```

---

## Fluxo: Sync Obsidian Vault (VPS ↔ GitHub)

```
Cron */5: git add -A && git commit && git push
    ↓
Cron * * * *: git pull (a cada 1 min)
    ↓
GitHub como intermediário
    ↓
Obsidian no PC ←→ GitHub ←→ VPS
```

### Para editar do PC
1. Instalar Obsidian no PC
2. Clonar repo: `git clone https://github.com/tecrochasales-a11y/obsidian-vault.git`
3. Editar notas localmente
4. Push para GitHub
5. Cron no VPS puxa automaticamente

---

## Fluxo: Nova Edge Function (Supabase Cloud)

```
Criar arquivo em supabase/functions/<nome>/index.ts
    ↓
Implementar lógica
    ↓
Testar local: supabase functions serve
    ↓
Deploy: npx supabase functions deploy <nome>
    ↓
Testar em produção
```

### Estrutura
```typescript
import { serve } from 'https://deno.land/std@0.177.0/http/server.ts'

serve(async (req) => {
  // lógica
  return new Response(JSON.stringify({ ok: true }))
})
```

---

## Fluxo: Debug 401 em Edge Function

```
401 retornado
    ↓
Verificar JWT (Authorization header)
    ↓
Edge Functions não recebem JWT automaticamente
    ↓
Solução: usar service_role key OU configurar JWT na função
    ↓
Ou: criar função sem auth (public)
```

### Soluções
1. `supabase.functions.invoke` com `Authorization: Bearer <token>`
2. JWT manual na função (`verifyJWT`)
3. Função pública (sem verificação)

---
*Última atualização: 2026-06-15*
