# Chatwoot — Kanban Integration

## URLs e Paths
- **Chatwoot:** https://chatwoot.rochasalesseguros.com.br
- **Kanban App:** `/var/www/chatwoot-kanban/`
- **Kanban URL:** https://chat.rochasalesseguros.com.br/kanban/

## Stack do Kanban
- React + Vite
- @hello-pangea/dnd (drag and drop)
- Supabase REST API

## Tabela de Mapeamento
`chatwoot_painel_map`

| Coluna | Tipo | Descrição |
|---|---|---|
| id | uuid | PK |
| chatwoot_contact_phone | text | Telefone (UNIQUE, NOT NULL) |
| chatwoot_conversation_id | text | ID conversa Chatwoot |
| painel_negocio_id | text | ID do negócio no Painel |
| painel_negocio_nome | text | Nome do negócio |
| etapa | text | Etapa atual no pipeline |
| created_at | timestamptz | |
| updated_at | timestamptz | |

## Colunas do Kanban
10 colunas espelhando as etapas do Painel:
1. Novos Leads (`novos_leads`)
2. Qualificação (`qualificacao`)
3. Remarketing (`remarketing`)
4. Reunião Agendada (`reuniao_agendada`)
5. Proposta Enviada (`proposta_enviada`)
6. Análise Seguradora (`analise_seguradora`)
7. Aguardando Pagamento (`aguardando_pagamento`)
8. Negócios Fechados (`fechado`)
9. Declinado (`declinado`)
10. Sem Interesse (`sem_interesse`)

## Problema Conhecido ⚠️
Kanban mostra apenas ~1000 cards no total (todos na coluna "Sem Interesse").
Possível causa: `limit=1000` default do PostgREST.

## API Fix Aplicado
```javascript
// GET com limit explícito
GET /rest/v1/chatwoot_painel_map?select=*&limit=10000&order=updated_at.desc
```

## Leads Inseridos
- Total: ~5,032 leads
- Leads sem telefone: usaram `SEMFONE-{hash}` como placeholder

## Integração com Chatwoot
- Token API: `2AfB8VhS79FTmxG1dL8774RJ`
- Webhook Edge Function: `chatwoot-webhook`

## Status (Jun 2026)
- ✅ Leads inseridos na tabela
- ✅ Kanban app deployado
- ⚠️ Exibição com problema (colunas vazias exceto "Sem Interesse")
- ❌ Kanban stages não batem com Painel stages

## Issues em Aberto
1. Kanban columns não distribuem leads corretamente
2. Stages do Kanban (`todo/in_progress/completed/remarketing`) ≠ Painel stages
3. 1,434 backlog do Chatwoot não importado

---
*Última atualização: 2026-06-13*
