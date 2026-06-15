# Chatwoot — Atendimento CRM

## URLs e Credenciais
- **URL:** https://chatwoot.rochasalesseguros.com.br
- **Porta:** 3000
- **Path VPS:** `/var/www/chatwoot`
- **Login:** `contato@rochasalesseguros.com.br` / `221109`

## Stack
- Chatwoot self-hosted (Ruby on Rails)
- PostgreSQL (container Docker)
- Redis (container Docker)
- Docker Compose em `/var/www/chatwoot/`

## Containers Docker
| Container | Status | Notas |
|---|---|---|
| `root-rails-1` | ✅ Running | Chatwoot principal |
| `root-sidekiq-1` | ✅ Running | Background jobs |
| `root-redis-1` | ✅ Running | Cache |
| `root-postgres-1` | ✅ Running | PostgreSQL + pgvector |
| `chatwoot-old` | ❌ Dead | Antigo, não usar |

## Arquitetura
```
Chatwoot (port 3000)
  ├── Rails app (root-rails-1)
  ├── Sidekiq workers (root-sidekiq-1)
  ├── PostgreSQL (root-postgres-1:5432)
  └── Redis (root-redis-1:6379)
```

## Kanban — Chatwoot + Painel do Corretor
- **Path:** `/var/www/chatwoot-kanban/`
- **Frontend:** `/var/www/chatwoot-kanban/dist/`
- **Script sync:** `/var/www/chatwoot-kanban/run-sync.sh`
- **Cron:** a cada 5 minutos
- **API:** Sincroniza leads do Painel do Corretor para o Chatwoot
- **Tabela:** `chatwoot_painel_map` (phone → Chatwoot conversation)

## ⚠️ Meta WhatsApp — SEPARADO DO COMERCIALRS!
Chatwoot tem configuração Meta WhatsApp **TOTALMENTE SEPARADA**.
- **ComercialRS:** webhook em `/var/www/comercialrs/data/meta_webhook_vps_service.py` (porta 8083)
- **Chatwoot:** outra configuração, outra pasta, **NÃO conflitar**

## Webhooks
- Kanban sync webhook: `/var/www/chatwoot-kanban/` → atualiza pipeline
- N8N automation: `https://n8n.rochasalesseguros.com.br/webhook/...`

## Integrações Ativas
| Integração | Status | Notas |
|---|---|---|
| WhatsApp (Meta) | ⚠️ Configuração própria | Separada do ComercialRS |
| Painel do Corretor | ✅ Sync ativo | A cada 5 min |
| n8n | ✅ Automação ativa | Port 5678 |
| Kanban board | ✅ Funcional | `/var/www/chatwoot-kanban/` |

## Comandos Úteis
```bash
# Status dos containers
docker ps | grep chatwoot

# Logs
docker logs root-rails-1 --tail 50
docker logs root-sidekiq-1 --tail 50

# Reiniciar
docker-compose -f /var/www/chatwoot/docker-compose.yml restart

# Acessar Rails console
docker exec -it root-rails-1 bundle exec rails c
```

## Status (Jun 2026)
- ✅ Container principal saudável (3+ dias uptime)
- ✅ Sidekiq a processar jobs
- ✅ Redis e PostgreSQL operacionais
- ⚠️ Kanban sync a funcionar — verificar se não há alertas duplicados

---
*Última atualização: 2026-06-15*
