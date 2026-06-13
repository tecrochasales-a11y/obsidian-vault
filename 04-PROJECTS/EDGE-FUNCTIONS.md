# Edge Functions — ComercialRS

## Lista Completa (26 funções)
Deployadas em: `https://dauftiqcvgaydddoxqhh.supabase.co/functions/v1/<nome>`

### Usadas no Sistema
| Função | Descrição |
|---|---|
| `dispatch-automation` | Dispara automações WhatsApp (templates Meta) |
| `meta-webhook` | Recebe webhooks da Meta WhatsApp API |
| `send-reminders` | Envia lembretes de reuniões |
| `chatwoot-webhook` | Recebe eventos do Chatwoot |
| `sync-painel-to-chatwoot` | Sincroniza estágios do Painel → Chatwoot labels |
| `crm-lead-sync` | Sincroniza leads do CRM externo |
| `sync-all-tasks` | Sincroniza todas as tarefas |
| `sync-profiles` | Sincroniza perfis |

### Admin
| Função | Descrição |
|---|---|
| `admin-reset-user-password` | Reseta senha de usuário |
| `admin-update-user-email` | Atualiza email de usuário |
| `update-profile-user-id` | Atualiza user_id em profile |
| `inspect-db-profiles` | Inspeta perfis no banco |
| `db-check-tables` | Verifica tabelas |
| `db-inspect` | Inspeção geral do banco |

### Utilitárias
| Função | Descrição |
|---|---|
| `firecrawl-extract-meeting` | Extrai dados de reunião |
| `resend-pending-meetings` | Reenvia reuniões pendentes |
| `update-responsibles` | Atualiza responsáveis |
| `update-tasks-created-by` | Atualiza created_by das tasks |
| `create-broker-links` | Cria links para corretores |
| `test-automation` | Testa automação |
| `test-network` | Testa conexão de rede |
| `test-webhook` | Testa webhook |
| `register-uazapi-webhook` | Registra webhook UAZAPI |
| `uazapi-status-webhook` | Status webhook UAZAPI |
| `meta-whatsapp-send` | Envia via WhatsApp Meta |

---

## dispatch-automation

### Função
Envia templates WhatsApp para corretores quando uma tarefa é criada/atualizada.

### Fluxo
1. Recebe payload com `taskId`, `eventType`, `responsibles[]`
2. Busca `automations_config` (singleton)
3. Verifica se automation está ativa
4. Para cada responsável: busca telefone
5. Envia template Meta via Graph API
6. Registra em `message_log` com `meta_message_id`

### Templates Enviados
| Template | Idioma | Params | Evento |
|---|---|---|---|
| `nova_reuniao_criada` | pt_BR | 6 | task_created |
| `reuniao_atualizada_com_sucesso` | pt_BR | 5 | task_updated |
| `lembrete_reuniao_em_30_minutos` | pt_BR | 3 | meeting_alert_30min |
| `1_reunioes_atrasadas_2` | pt_PT | 2 | overdue broadcast |
| `suas_reunioes_de_hoje` | pt_PT | 2 | daily briefing |
| `reunioes_sem_atualizao_ha_3_dias` | pt_PT | 2 | stale task alert |

### Anti-duplicação
Usa `message_log` para verificar se template já foi enviado.

---

## meta-webhook

### Função
Recebe respostas dos corretores via WhatsApp e atualiza status da tarefa.

### Verify Token
`1c5265...adab` (parcial)

### Endpoint
```
POST /functions/v1/meta-webhook
```

### Keywords de Status
```typescript
'in_progress':  ['iniciada', 'comecou', 'começou', 'andamento', 'em andamento', 'start', 'started']
'completed':   ['concluida', 'concluída', 'finalizada', 'done', 'completo', 'completa', 'fechada']
'remarketing': ['remarketing', 'remar', 'rmk', 'remercar']
```

### RPCs Usadas (SECURITY DEFINER)
- `log_incoming_message_rpc` — loga mensagem recebida
- `get_task_by_meta_message_id` — busca task pelo message_id
- `update_task_status_rpc` — atualiza status da task
- `get_automation_config_rpc` — busca config de automação

---

## send-reminders

### Função
Envia lembretes de reuniões.

### Suporta
- Meta WhatsApp
- UAZAPI

### Filtros
- Reuniões `todo` com `dueDate` no passado
- Diferentes tipos de alerta (30min, diário, stale)

---
*Última atualização: 2026-06-13*
