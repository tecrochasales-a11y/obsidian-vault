# Meta WhatsApp — Integração ComercialRS

## Credenciais Meta
| Campo | Valor |
|---|---|
| `meta_access_token` | `EAASNGf6ucRc...` (token longo) |
| `meta_phone_number_id` | `812251211981254` |
| `meta_waba_id` | `532448808878595` |
| `meta_business_id` | `1264409148885415` |

## API Endpoint
```
https://graph.facebook.com/v21.0/{phone_number_id}/messages
```

## Fluxo Bidirecional Completo

### Passo 1 — Envio (Broker → Corretor)
```
App (task created)
    ↓
dispatch-automation (Edge Function)
    ↓
Meta Graph API → WhatsApp broker
    ↓
message_log (meta_message_id + task_id)
```

### Passo 2 — Resposta (Broker → Sistema)
```
Broker responde "iniciada" / "concluída" / "remarketing"
    ↓
Meta webhook → meta-webhook (Edge Function)
    ↓
get_task_by_meta_message_id (RPC)
    ↓
update_task_status_rpc
    ↓
Envia confirmação ao corretor
```

## Tabelas

### message_log
```sql
CREATE TABLE public.message_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  meta_message_id TEXT,
  task_id UUID,
  responsible_id UUID,
  phone TEXT,
  status TEXT DEFAULT 'pending',
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### incoming_messages
```sql
CREATE TABLE public.incoming_messages (
  id UUID PRIMARY KEY,
  message_id TEXT UNIQUE,
  from_number TEXT,
  timestamp TIMESTAMPTZ,
  msg_type TEXT,
  body TEXT,
  source TEXT DEFAULT 'meta_webhook',
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

## RPCs (SECURITY DEFINER)

### log_incoming_message_rpc
Loga mensagem recebida.

### get_task_by_meta_message_id
Busca task pelo `meta_message_id` em `message_log`.

### update_task_status_rpc
Atualiza `tasks.status` baseado na keyword.

### get_automation_config_rpc
Busca `automations_config` singleton.

## ⚠️ REGRA CRÍTICA
**NUNCA tocar webhook no banco — conexão já está no site UAZAPI/Meta.**
Tocar = bloqueio do número WhatsApp.

## Bugs Conhecidos

### Bug 1 — message_log não existia
- `dispatch-automation` tentava POST para `/rest/v1/message_log`
- PostgREST rejeitou: `PGRST205` — tabela não encontrada
- **Fix:** SQL migration criando `message_log` e `incoming_messages`

### Bug 2 — RPCs não existiam
Todas as 6 RPCs não estavam no banco.
- **Fix:** SQL migration criando todas as RPCs

### Bug 3 — payload sem `id`
`AppDataContext.tsx` enviava payload sem `task.id`.
- **Fix:** Adicionado `id: task.id` aos payloads `task_created` e `task_updated`

### Bug 4 — Timer errado
Intervalo era 30 segundos → deveria ser 2h30.
- **Valor correto:** `150000`ms = 2.5 minutos (ou `9000000`ms = 2h30min)

## Status (Jun 2026)
- ✅ SQL migrations aplicadas
- ✅ RPCs criadas
- ✅ Frontend com payload corrigido
- ✅ `meta-webhook` funcionando
- ✅ `dispatch-automation` enviando

---
*Última atualização: 2026-06-13*
