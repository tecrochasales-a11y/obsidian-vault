# Base de Conhecimento — Rocha Sales

## Convenções Gerais

### Nomenclatura
- **Projetos:** `COMERCIALRS`, `DOCCORRETOR`, `CHATWOOT` (tudo maiúsculo)
- **Pastas VPS:** `/var/www/{projeto}/`
- **Branches:** `main` (sempre)
- **Commits:** em português, descritivos

### Formato de Datas
- ISO 8601: `YYYY-MM-DD` ou `YYYY-MM-DD HH:MM`
- Exemplo: `2026-06-15`, `2026-06-15 14:30`

### Formato de IDs
- Supabase: UUIDs em minúsculo (`dauftiqcvgaydddoxqhh`)
- WhatsApp: código país +55, formato `(XX) XXXXX-XXXX`

## Regras de Segurança

### Nunca fazer
- **NUNCA** dar `DROP TABLE` ou `DROP COLUMN` sem confirmar
- **NUNCA** commitar `.env` ou tokens no Git
- **NUNCA** mexer em webhook Meta WhatsApp sem confirmar — risco de bloqueio
- **NUNCA** confondir webhook do ComercialRS com Chatwoot

### Sempre verificar
- Backup antes de migrations
- Funcionamento antes E depois de cada mudança
- "Vai quebrar?" antes de mexer em produção

## Atalhos Rápidos

### VPS
```bash
# SSH rápido
alias vps='sshpass -p "Marcia19671951@" ssh -o StrictHostKeyChecking=no root@31.97.243.106'

# Status rápido
vps 'docker ps --format "table {{.Names}}\t{{.Status}}"'

# Logs nginx
vps 'tail -50 /var/log/nginx/error.log'
```

### Supabase
```bash
# CLI (já logado no projeto)
npx supabase status
npx supabase db push
npx supabase functions serve

# Deploy Edge Function
npx supabase functions deploy <nome>
```

### Git
```bash
# Push vault
cd /var/www/obsidian-vault
git add -A && git commit -m "update" && git push origin main

# Pull vault
cd /var/www/obsidian-vault && git pull origin main
```

## Stack Tecnológica

| Tecnologia | Versão | Uso |
|---|---|---|
| Node.js | v20.20.2 | Builds TanStack Start |
| NVM | latest | Gerenciamento Node |
| PostgreSQL | 15+ | Supabase Cloud |
| Docker | latest | Containers VPS |
| nginx | latest | Reverse proxy + SSL |
| certbot | latest | SSL Let's Encrypt |

## PostgREST — Sintaxe de Filtros

| Filtro | Sintaxe | Exemplo |
|---|---|---|
| Igual | `column=value` | `status=todo` |
| Nulo | `column=is.null` | `deleted_at=is.null` |
| NÃO nulo | `column=not.is.null` | `updated_at=not.is.null` |
| Contém | `column=ilike.*valor*` | `name=ilike.*joao*` |
| Maior que | `column=gt.value` | `id=gt.100` |
| In | `column=in.(a,b,c)` | `status=in.(todo,done)` |

**ATENÇÃO:** `is=column.null` dá 400 — usar `column=is.null`.

## Supabase — Tabelas Principais

### ComercialRS
- `profiles` — perfis de usuário (user_id → auth.users)
- `team_members` — membros da equipa
- `tasks` — tarefas (created_by=profile.id, responsible_id=team_members.id)
- `meetings` — reuniões
- `broker_links` — links de corretor
- `alert_throttle` — throttle anti-spam

### DocCorretor
- `clients` — clientes
- `documents` — documentos
- `folders` — pastas (com client_id e folder_id)
- `storage_files` — arquivos no Supabase Storage

### Chatwoot
- `chatwoot_painel_map` — mapeamento phone → conversation_id

## WordPress (Legacy)
- **URL:** https://rochasalesseguros.com.br
- **Path:** `/var/www/rochasales/`
- **Plugins:** Contact Form 7, Yoast SEO
- **Theme:** Theme padrão
- **Status:** Legado — manter mas não desenvolver

## n8n — Automações
- **URL:** https://n8n.rochasalesseguros.com.br
- **Porta:** 5678
- **Credenciais:** guardadas no n8n credential store
- **Workflows ativos:**Chatwoot ↔ Painel do Corretor sync

## Erros Comuns e Soluções

### "Invalid API key" 401
- Verificar se `.env` tem `SUPABASE_URL` e `SUPABASE_ANON_KEY` corretos
- Para Edge Functions: usar service_role key ou configurar JWT na função

### "EADDRINUSE" no Node
- Já existe processo na porta — matar com `lsof -i :PORTA` e `kill PID`

### "Cannot read properties of undefined"
- Provavelmente `.split()` ou `.map()` em valor null/undefined
- Adicionar guard: `valor ? valor.split() : ''`

### nginx 502
- Backend não está a responder — verificar se serviço está up
- `systemctl status <servico>`
- `journalctl -u <servico> -f`

### SSL expira
- `certbot renew --dry-run`
- Verificar se certbot cron está ativo

---
*Última atualização: 2026-06-15*
