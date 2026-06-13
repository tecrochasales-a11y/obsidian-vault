# Chatwoot — Kanban — Detalhes Técnicos

## URLs
- **Chatwoot:** https://chatwoot.rochasalesseguros.com.br
- **Kanban:** https://chat.rochasalesseguros.com.br/kanban/

## Paths
- **App:** `/var/www/chatwoot-kanban/`
- **Build:** `/var/www/chatwoot-kanban/dist/`

## Stack
- React + Vite
- @hello-pangea/dnd (drag and drop)
- Supabase REST API

## Build
```bash
source ~/.nvm/nvm.sh && nvm use 20 && npm run build
```
⚠️ **Node 18 causa build incompleto — usar Node 20**

## API Fetch (com paginação)
```typescript
// Fetch paginado — necessário por causa do limit=1000 do PostgREST
while (true) {
  const resp = await fetch(
    `${SUPABASE_URL}/rest/v1/chatwoot_painel_map?select=*&limit=${pageSize}&offset=${offset}&order=updated_at.desc`,
    ...
  );
  const batch: ChatwootPainelMap[] = await resp.json();
  allData.push(...batch);
  if (batch.length < pageSize) break;
  offset += pageSize;
}
```

## Tabela: chatwoot_painel_map

```sql
CREATE TABLE public.chatwoot_painel_map (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  chatwoot_contact_phone TEXT UNIQUE NOT NULL,
  chatwoot_conversation_id TEXT,
  painel_negocio_id TEXT,
  painel_negocio_nome TEXT,
  etapa TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

## Etapas do Pipeline (Painel)
| Nome | Valor em `etapa` |
|---|---|
| Novos Leads | `novos_leads` |
| Qualificação | `qualificacao` |
| Remarketing | `remarketing` |
| Reunião Agendada | `reuniao_agendada` |
| Proposta Enviada | `proposta_enviada` |
| Análise Seguradora | `analise_seguradora` |
| Aguardando Pagamento | `aguardando_pagamento` |
| Fechado | `fechado` |
| Declinado | `declinado` |
| Sem Interesse | `sem_interesse` |

## UUIDs das Etapas (Painel API)
| Etapa | UUID |
|---|---|
| Novos Leads | `60008110-e9b1-4512-b514-ab1539ea1a1e` |
| Qualificação | `885d4f95-99e5-42a0-8150-b494c42c0e34` |
| Remarketing | `6699997d-6978-4679-8b4c-eaab9917fefd` |
| Reunião Agendada | `4646a3e9-b21a-4339-b75b-6997fa3dbb76` |
| Proposta Enviada | `4e61480b-b4dc-4b80-b0c8-dd3ddf8e43b2` |
| Análise Seguradora | `29511da1-1e40-4e9c-8b14-a268ccbdc1d0` |
| Aguardando Pagamento | `2584da59-4c1e-4fcb-8cba-8374acd896a7` |
| Fechado | `df8db361-5b4c-4e66-baac-afcfd9fbe50a` |
| Declinado | `70150b84-6864-4cff-aae1-83195a182d8d` |
| Sem Interesse | `63f4b9df-55d6-4ff0-a42c-af5ff2ff367b` |

## Chatwoot API
- **Token:** `2AfB8VhS79FTmxG1dL8774RJ`
- **Account:** 1
- **Endpoint:** `https://chat.rochasalesseguros.com.br/api/v1/`

## Bulk Insert (via Supabase REST)
```bash
# Batch de 500 registros
POST /rest/v1/chatwoot_painel_map
Headers: Authorization, Content-Type, Prefer: resolution=mergeDuplicates

# Leads sem telefone usaram placeholder
SEMFONE-{hash_email}
```

## Issues em Aberto

### Issue 1 — "Aguardando Pagamento" mostra 0
Mesmotabela tendo 2 registros com `etapa='aguardando_pagamento'`.
Possível causa: cache do navegador ou mismatch no valor da etapa.

### Issue 2 — Drag-and-drop não atualiza Painel
`PATCH /api/crm/negocios/{id}` retorna 500.
Sincronização bidirecional incompleta.

### Issue 3 — 143 leads sem telefone excluídos
Restrição `NOT NULL + UNIQUE` em `chatwoot_contact_phone`.
Usaram placeholder mas 143 não tinham nem email.

## Containers
| Container | Status |
|---|---|
| `chatwoot` | ✅ Health |
| `chatwoot-old` | ⚠️ Dead (não usar) |

---
*Última atualização: 2026-06-13*
