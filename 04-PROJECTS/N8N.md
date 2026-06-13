# n8n — Automações

## URLs e Credenciais
- **URL:** https://n8n.rochasalesseguros.com.br
- **Porta:** 5678
- **Path VPS:** `/var/www/n8n`

## Stack
- n8n self-hosted (Node.js)
- PostgreSQL (banco de workflows)
- Redis

## Status (Jun 2026)
- ✅ Rodando normalmente

## Uso Principal
Automações de workflows, incluindo:
- Sincronização CRM → Chatwoot
- Notificações WhatsApp
- Alertas de reuniões
- Sincronização de tarefas

## Automações Conectadas
- `sync-painel-to-chatwoot` — sincroniza painel do corretor com Chatwoot
- `crm-lead-sync` — sincroniza leads do CRM externo
- Webhooks para dispatch de automações

## Segurança
- Workflows armazenam credenciais encriptadas
- JWT extraído do banco para GraphQL

## Recursos
- Workflows visuais
- Integrações com Supabase
- Webhooks HTTP
- JavaScript expressions
- Multi-step workflows

---
*Última atualização: 2026-06-13*
