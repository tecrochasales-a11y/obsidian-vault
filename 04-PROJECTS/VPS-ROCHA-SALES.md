# VPS Rocha Sales — 31.97.243.106

## Credenciais
- **IP:** 31.97.243.106
- **SSH:** root / Marcia19671951@
- **Método SSH:** `sshpass -p 'Marcia19671951@' ssh root@31.97.243.106`

## Serviços Ativos (Jun 2026)

### Docker Containers
| Container | Status | Porta | Descrição |
|---|---|---|---|
| Chatwoot | ✅ Health | 3000 | Atendimento |
| n8n | ✅ Running | 5678 | Automações |
| root-postgres | ✅ Running | 5432 | PostgreSQL |
| root-redis | ✅ Running | 6379 | Redis |
| ollama (d0xb) | ✅ Running | 11434 | LLMs locais |
| deploy-vps-functions | ✅ Running | 8000 | Edge Functions |
| deploy-vps-imgproxy | ✅ Running | - | Proxy de imagens |
| Chatwoot (old) | ⚠️ Dead | - | Versão antiga |
| evolution-api | ❌ Removed | - | Removido Jun/2026 |
| evolution-postgres | ❌ Removed | - | Removido Jun/2026 |
| evolution-redis | ❌ Removed | - | Removido Jun/2026 |

### Sites Nginx
| Domínio | Status | Descrição |
|---|---|---|
| rochasalesseguros.com.br | ✅ | Site principal LP |
| ComercialRS | ✅ | CRM de vendas (:3010) |
| documentos.rochasalesseguros.com.br | ✅ | DocCorretor (:3010) |
| obsidian.rochasalesseguros.com.br | ✅ | **Vault Obsidian** |
| chatwoot.rochasalesseguros.com.br | ✅ | Chatwoot (:3000) |
| n8n.rochasalesseguros.com.br | ✅ | n8n (:5678) |

### Portas em Uso
| Porta | Serviço |
|---|---|
| 80/443 | Nginx |
| 3000 | Chatwoot |
| 3002 | Meta webhook |
| 3010 | ComercialRS + DocCorretor |
| 43502 | OpenClaw |
| 5432 | PostgreSQL |
| 5678 | n8n |
| 6379 | Redis |
| 8000 | deploy-vps (Edge Functions) |
| 8083 | Meta WhatsApp (ComercialRS) |
| 11434 | Ollama |
| 32769 | Unknown (Ollama?) |

## Limpeza Feita (Jun 2026)
- ❌ Evolution API + containers + volumes
- ❌ Volume `ollama-nef5_ollama` (15GB)
- ❌ Volume `ollama_data` (1.8GB)
- ✅ ~17GB liberados

## A Investigar
- `/var/www/var/` (607MB) — propósito?
- OpenClaw (`/var/www/var/` na porta 43502) — em uso?
- LP Backend (porta 8001) — em uso?
- `root-postgres` + `pgvector` — mesmo que `deploy-vps-postgres`?
- Imagens Docker não usadas (~40GB)

## Backups
- Obsidian vault → GitHub (a cada 5 min)
- Docker volumes importante manter

---
*Última atualização: 2026-06-13*
