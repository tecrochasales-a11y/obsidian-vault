# Supabase Self-Hosted — deploy-vps

## Status (Jun 2026)
**⚠️ QUEBRADO — Não usar**

Container `deploy-vps-kong-1` estava parado por ~18h.
Stack foi reiniciada mas `auth` e `rest` não conectam ao banco.

## Localização
- **Path:** `/docker/deploy-vps/`
- **Docker compose:** `/docker/deploy-vps/docker-compose.yml`

## Containers
| Container | Status | Porta | Descrição |
|---|---|---|---|
| `deploy-vps-kong-1` | ✅ Running | 8000 | Kong API Gateway |
| `deploy-vps-db-1` | ✅ Running | 5433 | PostgreSQL 15 |
| `deploy-vps-auth-1` | ❌ Crashed | - | GoTrue (auth) |
| `deploy-vps-rest-1` | ❌ Crashed | - | PostgREST |
| `deploy-vps-storage-1` | ✅ Running | - | Storage |
| `deploy-vps-meta-1` | ✅ Running | - | Postgres Meta |
| `deploy-vps-studio-1` | ✅ Running | 8080 | Postgres Studio |
| `deploy-vps-functions-1` | ✅ Running | 8000 | Edge Functions |
| `deploy-vps-imgproxy-1` | ✅ Running | - | Image proxy |

## Problema: PostgreSQL Auth
```
FATAL: password authentication failed for user "postgres"
```

### Causa
- `POSTGRES_PASSWORD=Rochasales2024` está no `.env`
- Mas containers internos recebem string vazia na conexão
- `pg_hba.conf` com `scram-sha-256` para subnet Docker `172.23.0.0/16`
- Modificações no `pg_hba.conf` não estão sendo aplicadas

### Solução Parcial
Modificar `pg_hba.conf` para `trust` na subnet Docker:
```
host all all 172.23.0.0/16 trust
```

## Variáveis de Ambiente
| Variável | Valor |
|---|---|
| `POSTGRES_PASSWORD` | `Rochasales2024` |
| `POSTGRES_DB` | `postgres` |
| `JWT_SECRET` | (do .env) |

## Rede
- **Network:** `deploy-vps_default`
- **Subnet:** `172.23.0.0/16`

## Decisão do Usuário
**Pendente:** apakah fix atau remove?

---

## Histórico de Tentativas
1. Reiniciou todos os containers → auth/rest ainda crash
2. Modificou `pg_hba.conf` → não pegou
3. Tentou `trust` → não funcionou

---

## Nota
Este stack é separado do **Supabase Cloud** (`dauftiqcvgaydddoxqhh`).
O ComercialRS e DocCorretor usam Supabase Cloud, não este self-hosted.

---
*Última atualização: 2026-06-13*
