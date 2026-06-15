# Painel do Corretor — Trindade Tecnologia

## Visão Geral
Sistema externo (Trindade Tecnologia) que gerencia leads e pipelines de vendas. ComercialRS usa como fonte de leads para o Chatwoot.

## URLs
- **GraphQL:** `https://api.paineldocorretor.net/graphql`
- **Auth:** `https://auth.paineldocorretor.com.br/oauth/token`
- **Dashboard:** https://paineldocorretor.com.br

## Autenticação (Auth0 JWT)
```http
POST https://auth.paineldocorretor.com.br/oauth/token
Content-Type: application/json

{
  "grant_type": "http://auth0.com/oauth/grant-type/password-realm",
  "realm": "Username-Password-Authentication",
  "username": "davi.freire@rochasalesseguros.com.br",
  "password": "***",
  "client_id": "MtCJ0bBdECwihpKoeTBf9L0E6JvCEx24",
  "audience": "https://paineldocorretor.com.br/api/"
}
```

**ATENÇÃO:** Token expira periodicamente. Renovar via POST /oauth/token.

## Headers Obrigatórios (GraphQL)
```
X-Tenant: a1af7a2f-7ec6-40cd-b257-86c045875979
Authorization: Bearer <token>
Content-Type: application/json
```

## Query Principal — negociosElastic
```graphql
query FooBarQuery {
  negociosElastic(take: 10, skip: 0) {
    id
    nome
    etapa
    contato {
      id
      nome
      telefones
      emails
    }
  }
}
```

**IMPORTANTE:** Usar `negociosElastic` — não `negocios` (deprecated).

## Etapas do Pipeline
| Etapa | Valor no banco |
|---|---|
| Novos Leads | `novos_leads` |
| Qualificação | `qualificacao` |
| Remarketing | `remarketing` |
| Reunião Agendada | `reuniao_agendada` |
| Proposta Enviada | `proposta_enviada` |
| Análise Seguradora | `analise_seguradora` |
| Aguardando Pagamento | `aguardando_pagamento` |
| Negócios Fechados | `fechado` |
| Declinado | `declinado` |
| Sem Interesse | `sem_interesse` |

## Limitações Conhecidas
- `contato.telefones` retorna `[]` para **todos** — usar email como join key
- API pode retornar **500** (erro interno do Painel) — tratar com retry
- Token expira — monitorar e renovar automaticamente

## Total de Leads
~5,175 leads distribuídos nas 10 etapas

## Integração com ComercialRS
- Sincronizado via Edge Function `sync-painel-to-chatwoot`
- Mapeado na tabela `chatwoot_painel_map`
- Phone como chave única
- Leads sem telefone: `SEMFONE-{hash(email)}`

## Integração com Chatwoot
- Script: `/var/www/chatwoot-kanban/run-sync.sh`
- Cron: `*/5 * * * *`
- Sincroniza etapa do Painel → Chatwoot conversation

## Comandos Úteis
```bash
# Testar GraphQL
curl -s -X POST https://api.paineldocorretor.net/graphql \
  -H "X-Tenant: a1af7a2f-7ec6-40cd-b257-86c045875979" \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"query":"{ negociosElastic(take:1,skip:0){id nome etapa}}"}'

# Ver token atual
# (guardado no script de sync ou variável de ambiente)
```

---
*Última atualização: 2026-06-15*
