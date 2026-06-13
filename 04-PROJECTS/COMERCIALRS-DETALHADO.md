# ComercialRS — CRM de Vendas

## URLs e Credenciais
- **URL:** https://comercialrs.rochasalesseguros.com.br
- **Path VPS:** `/var/www/comercialrs`
- **Porta:** 3010
- **Build:** TanStack Start (React + Vite)
- **Supabase:** `dauftiqcvgaydddoxqhh` (West US Oregon)

## Stack Técnica
- Frontend: React + TanStack Router + Tailwind + shadcn/ui
- Backend: Supabase Cloud
- Auth: Google OAuth (via Lovable + GoTrue local porta 9999)
- WhatsApp: Meta WhatsApp Business API

## Nginx — Proxy Reverso
```
/rest/v1/*     → https://dauftiqcvgaydddoxqhh.supabase.co (apikey anônima embedada)
/storage/v1/*  → https://dauftiqcvgaydddoxqhh.supabase.co
/functions/v1/* → https://dauftiqcvgaydddoxqhh.supabase.co
/auth/v1/*     → http://127.0.0.1:9999 (GoTrue local)
```

## Tabelas Principais (Supabase)

### automations
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK |
| key | text | Identificador único |
| name | text | Nome |
| description | text | Descrição |
| icon | text | Ícone |
| status | boolean | Ativo/inativo |
| last_run_at | timestamptz | Último execução |

### automations_config
Singleton global (`user_id IS NULL`). Armazena:
- `crm_auto_sync` — toggle sync automática server-side
- `crm_last_synced_at` / `crm_last_sync_count` / `crm_last_sync_status`
- `provider` — `n8n` ou `meta`
- Meta credentials: `meta_access_token`, `meta_phone_number_id`, `meta_waba_id`, `meta_business_id`
- UAZAPI: `uazapi_url`, `uazapi_token`, `uazapi_phone`
- Webhook: `webhook_url`, `webhook_mode`

### profiles
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK — mesmo que auth.users.id |
| user_id | uuid | FK → auth.users.id |
| full_name | text | Nome completo |
| email | text | Email |
| role | text | Papel |
| onboarding_done | boolean | Onboarding completo |

### team_members
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK — DIFERENTE de profiles.id |
| profile_id | uuid | FK → profiles.id |
| name | text | Nome |
| phone | text | Telefone |
| role | text | Papel |

### tasks
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK |
| created_by | uuid | FK → profiles.id |
| responsible_id | uuid | FK → team_members.id |
| title | text | Título |
| status | text | Status: todo, in_progress, completed, remarketing |
| due_date | timestamptz | Data de vencimento |

### crm_lead
Espelho do CRM externo (sincronizado por n8n/cron)

### message_log
Armazena `meta_message_id`, `task_id`, `responsible_id`, `phone`, `status`
Usado para anti-duplicação de envios WhatsApp

### incoming_messages
Armazena respostas recebidas via webhook Meta

### alert_throttle
Anti-spam: evita envios duplicados (janela 30min)
Campos: `signature`, `event_type`, `target_phone`, `responsible_id`, `task_ids`, `expires_at`

## Edge Functions (20+ ativas)
```
admin-reset-user-password
admin-update-user-email
crm-lead-sync
crm-mirror-health
crm-sales-pipeline
crm-sales-sync
db-check-tables
db-inspect
dispatch-automation
firecrawl-extract-meeting
init-whatsapp-table
inspect-db-profiles
resend-pending-meetings
send-reminders
sync-all-tasks
sync-painel-to-chatwoot
sync-profiles
test-network
uazapi-status-webhook
update-responsibles
update-profile-user-id
create-broker-links
chatwoot-webhook
meta-webhook
test-webhook
```

## Automations Padrão
| Key | Nome | Trigger |
|---|---|---|
| whatsapp_reminders | Lembrete de Reunião | 30min antes |
| risk_alerts | Alerta de Reunião Atrasada | Quando atrasa |
| daily_briefing | Briefing Diário | 10h, 13h, 17h |
| weekly_closing | Encerramento Semanal | Sexta |

## WhatsApp — Meta Business API

### Fluxo Bidirecional
1. `dispatch-automation` envia template Meta → corretor
2. Corretor responde "iniciada" / "concluída" / "remarketing"
3. Meta envia webhook → `meta-webhook` Edge Function
4. Extrai `meta_message_id` → busca `task_id`
5. Atualiza status da task

### Keywords de Status
- **in_progress:** iniciada, começou, andamento, start, started
- **completed:** concluída, finalizada, done, fechada
- **remarketing:** remarketing, remar, rmk, remercar

### ⚠️ REGRA CRÍTICA
**NÃO tocar webhook no banco de dados — conexão já está no site UAZAPI/Meta.**
Tocar = bloqueio do número WhatsApp.

## Bugs Corrigidos (2026)
- Date filter null guard em Dashboard.tsx
- Alert throttle (30min anti-duplicado)
- useMeetingAlerts.ts interval 30s → 150s (2h30)

## Build e Deploy
```bash
cd /var/www/comercialrs
nvm use 20
npm run build
# Output: /var/www/comercialrs/dist/
```

---
*Última atualização: 2026-06-13*
