# Painel do Corretor — API GraphQL

## Visão Geral
Sistema externo (Trindade Tecnologia) que gerencia leads e pipelines de vendas.

## URLs
- **GraphQL:** `https://api.paineldocorretor.net/graphql`
- **Auth:** `https://auth.paineldocorretor.com.br/oauth/token`
- **Dashboard:** Painel do Corretor (Trindade)

## Autenticação (Auth0)
```http
POST https://auth.paineldocorretor.com.br/oauth/token
Content-Type: application/json

{
  "grant_type": "http://auth0.com/oauth/grant-type/password-realm",
  "realm": "Username-Password-Authentication",
  "username": "davi.freire@rochasalesseguros.com.br",
  "password": "131415pi",
  "client_id": "MtCJ0bBdECwihpKoeTBf9L0E6JvCEx24",
  "audience": "https://paineldocorretor.com.br/api/"
}
```

## Query Principal (negociosElastic)
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

**IMPORTANTE:** Usar `negociosElastic` — não `negocios`.

## Headers Obrigatórios
```
X-Tenant: a1af7a2f-7ec6-40cd-b257-86c045875979
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

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
- `contato.telefones` retorna `[]` para todos — usar email como join key
- API pode retornar 500 (erro interno do Painel)

## Total de Leads
~5,175 leads distribuídos nas 10 etapas

## Integração com ComercialRS
- Sincronizado via Edge Function `sync-painel-to-chatwoot`
- Mapeado na tabela `chatwoot_painel_map`
- Phone como chave única (exceto leads sem fone = `SEMFONE-{hash}`)

---
*Última atualização: 2026-06-13*
