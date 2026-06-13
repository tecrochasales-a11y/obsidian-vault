# Chatwoot — Atendimento

## URLs e Credenciais
- **URL:** https://chatwoot.rochasalesseguros.com.br
- **Porta:** 3000
- **Path VPS:** `/var/www/chatwoot`

## Stack
- Chatwoot self-hosted (Ruby on Rails)
- PostgreSQL
- Redis

## Status (Jun 2026)
- ✅ Container rodando (saudável)
- ⚠️ Container antigo (old) está dead — não usar

## Integrações
- WhatsApp via configuração própria (separada do ComercialRS!)
- Webhooks para automação
- Kanban integrado

## Containers Docker
| Container | Status |
|---|---|
| chatwoot | ✅ Health |
| chatwoot-old | ⚠️ Dead |

## Recursos
- Caixa de entrada unificada
- Chat em tempo real
- Relatórios
- Integrações com WhatsApp

## ⚠️ IMPORTANTE
Chatwoot tem configuração Meta WhatsApp **TOTALMENTE SEPARADA** do ComercialRS.
- ComercialRS: webhook em `/var/www/comercialrs/data/meta_webhook_vps_service.py` (porta 8083)
- Chatwoot: outra configuração, outra pasta

**NÃO confundir os dois sistemas.**

---
*Última atualização: 2026-06-13*
