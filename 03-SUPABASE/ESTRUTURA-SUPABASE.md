# Supabase — Estrutura do Projeto

## Projeto
- **Ref:** `dauftiqcvgaydddoxqhh`
- **URL:** https://supabase.com/dashboard/project/dauftiqcvgaydddoxqhh
- **Compartilhado por:** ComercialRS + DocCorretor

## Estrutura de Identidades (ComercialRS)

```
auth.users.id
    ↓ (user_id)
profiles.user_id = profiles.id
    ↓ (profile.id)
team_members.id (DIFERENTE de profiles.id!)
    ↓
tasks.created_by = profiles.id
tasks.responsible_id = team_members.id
```

## Tables Principais

### profiles
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK — mesmo valor que auth.users.id |
| user_id | uuid | FK → auth.users.id |
| name | text | Nome do usuário |
| role | text | papel no sistema |
| created_at | timestamptz | |

### team_members
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK — diferente de profiles.id! |
| profile_id | uuid | FK → profiles.id |
| name | text | Nome |

### tasks (ComercialRS)
| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK |
| created_by | uuid | FK → profiles.id |
| responsible_id | uuid | FK → team_members.id |
| title | text | Título da tarefa |
| status | text | Status |

## Service Role Key
Disponível para operações de admin (confirmar emails, criar users).
**NUNCA expor no frontend.**

## management API (sbp_...)
Acesso **LIMITADO** — não permite gerenciar usuários diretamente.

## Como Corrigir Conta de Usuário
1. Localizar profile pelo email
2. Atualizar `profile.user_id` para o novo `auth.users.id`

## Storage
- Bucket principal: documentos e arquivos
- Permissões via RLS

## Spam Fix (Jun 2026)
- `task_alerts.py` wrapper lock 9000s (2h30)
- `message_log` check anti-spam

---
*Última atualização: 2026-06-13*
