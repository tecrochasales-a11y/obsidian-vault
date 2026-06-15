# Decisões Arquiteturais

## Meta WhatsApp — SEPARAÇÃO CRÍTICA
**Data:** Jun 2026

ComercialRS e Chatwoot têm configurações Meta WhatsApp **100% separadas**.

### ComercialRS
- Webhook próprio
- Arquivo: `/var/www/comercialrs/data/meta_webhook_vps_service.py`
- Porta: 8083
- Recebe botões e atualiza tarefas
- **NÃO** chama Chatwoot API

### Chatwoot
- Configuração separada
- Outra pasta, outro webhook
- **NÃO** conflitar com ComercialRS

---

## Fluxo "Link para Corretor" — DocCorretor
**Data:** Maio 2026

O fluxo foi projetado para criar a pasta ANTES do externo preencher:
1. Admin clica → token + pasta criada com `client_id=null`
2. Link gerado com `folder_id`
3. Externo preenche
4. API atualiza com `client_id`

**Este fluxo é inalterável** sem autorização do Tecrocha.

---

## Migração Obsidian
**Data:** Jun 2026

Decisão: Obsidian como cérebro central do projeto, substituindo gradualmente o Supabase como repositório de memória operacional.

### Arquitetura
- Vault: `/var/www/obsidian-vault` (VPS)
- Site: https://obsidian.rochasalesseguros.com.br
- Sync: GitHub (tecrochasales-a11y/obsidian-vault)
- Push: a cada 5 min
- Pull: a cada 1 min

### Estratégia
- Zero dados removidos do Supabase
- Supabase permanece ativo
- Obsidian é inicialmente réplica
- Transição operacional só após validação completa

---

## Evolução API — Remoção
**Data:** Jun 2026

Removida do VPS por não estar em uso:
- Containers deletados
- Volumes removidos
- ~17GB liberados

---

## Builds Node — Sempre NVM
**Data:** Jun 2026

Antes de qualquer build Node.js na VPS, usar:
```bash
nvm use 20
```
Versão correta: v20.20.2

---

## CommercialRS — Sistema Separado
**Data:** Jun 2026

ComercialRS é completamente separado de:
- n8n
- Chatwoot
- Kanban

Cada um tem sua própria pasta, configurações e workflows.

---
*Última atualização: 2026-06-13*

## Obsidian Vault — Atualização Fase 3 (Jun 2026)
**Data:** Jun 2026

Vault populado com conteúdo da memória Hermes:
- 01-MEMORY atualizada com user profile completo
- 04-PROJECTS: Chatwoot e Painel Corretor detalhados
- 05-WORKFLOWS: workflows práticos documentados
- 08-KNOWLEDGE: base de conhecimento criada

### Decisão: Site como Visualização, Não Edição
- Browser view: https://obsidian.rochasalesseguros.com.br (só leitura)
- Obsidian app no PC: edição completa via Git sync
- Sync: push a cada 5 min, pull a cada 1 min

### Decisão: Vault Mantém Frontmatter Simples
- Sem formato Obsidian Publish pago
- Custom Node.js server para visualização
- GitHub como intermediário de sync

## VPS — Acesso e Gestão
**Data:** Jun 2026

### Credenciais
- IP: 31.97.243.106
- User: root
- Senha: Marcia19671951@

### Pastas Protegidas (não deletar)
- /var/www/documentos
- /var/www/app
- /var/www/rochasales
- /var/www/chatwoot
- /var/www/evolution-manager
- /var/www/n8n
- /var/www/html
- /var/www/traefik

### Volumes Removidos (Jun 2026)
- evolution_evolution_postgres (48MB)
- ollama-nef5_ollama (15GB)
- ollama_data (1.8GB)
- Total freed: ~17GB

## DocCorretor — Fluxo Link para Corretor (Confirmado)
**Data:** Jun 2026

Fluxo INALTERÁVEL sem autorização do Tecrocha:
1. Admin clica → token + pasta criada com client_id=null
2. Link gerado com folder_id
3. Externo preenche
4. API atualiza pasta com client_id

Qualquer alteração需 aprovação explícita.

## Spam Protection — Alert Throttle
**Data:** Mai 2026

Tabela alert_throttle implementada:
- signature: hash de event_type + target_phone
- expires_at: 2h30 após criação
- Lock 9000s em task_alerts.py

Objetivo: evitar alertas duplicados para o mesmo corretor.

---
*Última atualização: 2026-06-15*
