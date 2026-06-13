# Incidentes e Bugs Resolvidos

## WhatsApp Bloqueado — UAZAPI (Mai 2026) 🔴 CRÍTICO
**Problema:** Número WhatsApp bloqueado por 5 horas.

**Causa:** Webhook configurado no banco de dados quando já estava configurado no site UAZAPI. Duplicação de envios = spam = bloqueio.

**Regra estabelecida:**
> NUNCA tocar webhook no banco — conexão já está no site UAZAPI/Meta. Tocar = bloqueio.

**Solução:** Reconectado token no site do ComercialRS.

---

## Alertas Duplicados (Mai 2026)
**Problema:** Corretores recebiam mensagens duplicadas de alerta.

**Causa:** Sistema verificava a cada 30min sem throttle.

**Solução:** Implementado throttle de 30min em `alert_throttle` table.

---

## Tela Preta — Filtro de Datas (Mai 2026)
**Problema:** Clicar para definir período de data causava tela preta.

**Causa:** `t.dueDate.split('T')[0]` sem null-check em Dashboard.tsx.

**Solução:** Added guard: `t.dueDate ? t.dueDate.split('T')[0] : ''`

---

## Accesso Maju (Mai 2026) ⚠️ PENDENTE
**Problema:** `maria.giulha@rochasalesseguros.com.br` não conseguia acessar.

**Causa:** Conta criada mas email nunca confirmado (`email_confirmed_at = NULL`).

**Status:** Resolução pendente — necesita senha reset via `admin-reset-user-password`.

---

## Botão "Iniciada" Não Parava Alertas (Mai 2026)
**Problema:** Clicar "Iniciada" atualizava task mas vendedor continuava recebendo alertas.

**Causa:** Task já estava "agendada" no ciclo do cron antes do clique.

**Solução necessária:** `task_alerts.py` precisa excluir tasks com `status IN ('in_progress', 'completed')`.

---

## Meta Webhook — Timestamp Vazio (Mai 2026)
**Problema:** Erro `invalid input syntax for type bigint: ""` no log.

**Causa:** `timestamp` vazio sendo passado no insert de `incoming_messages`.

**Impacto:** Nenhum — task era atualizada corretamente.

---

## DocCorretor — Pastas Vazias (Jun 2026)
**Problema:** App mostrava lista vazia de clientes.

**Causa:** Migração Drive criou registros em `documents` mas não em `clients`.

**Solução:** Migração completa refeita — 138 docs, 138 arquivos, 12 clientes.

---

## PostgREST Null Filter — Bug Silencioso (Jun 2026)
**Problema:** Filtro `is=column.null` retornava 400 sem mensagem.

**Solução:** Sintaxe correta: `column=is.null`

---
*Última atualização: 2026-06-13*
