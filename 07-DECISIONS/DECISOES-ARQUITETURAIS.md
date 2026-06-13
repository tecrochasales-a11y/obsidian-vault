# Decisões Arquiteturais

## Meta WhatsApp — SEPARAÇÃO CRÍTICA
**Data:** Jun 2026

ComercialRS e Chatwoot têm configurações Meta WhatsApp **100% separadas**.

### ComercialRS
- Webhook próprio
- Arquivo: `/var/www/comercialrs/data/meta_webhook_vps_service.py`
- Porta: 8083
- Recebe botões e atualiza tarefas
- **NÃO** chama Chatwoot API

### Chatwoot
- Configuração separada
- Outra pasta, outro webhook
- **NÃO** conflitar com ComercialRS

---

## Fluxo "Link para Corretor" — DocCorretor
**Data:** Maio 2026

O fluxo foi projetado para criar a pasta ANTES do externo preencher:
1. Admin clica → token + pasta criada com `client_id=null`
2. Link gerado com `folder_id`
3. Externo preenche
4. API atualiza com `client_id`

**Este fluxo é inalterável** sem autorização do Tecrocha.

---

## Migração Obsidian
**Data:** Jun 2026

Decisão: Obsidian como cérebro central do projeto, substituindo gradualmente o Supabase como repositório de memória operacional.

### Arquitetura
- Vault: `/var/www/obsidian-vault` (VPS)
- Site: https://obsidian.rochasalesseguros.com.br
- Sync: GitHub (tecrochasales-a11y/obsidian-vault)
- Push: a cada 5 min
- Pull: a cada 1 min

### Estratégia
- Zero dados removidos do Supabase
- Supabase permanece ativo
- Obsidian é inicialmente réplica
- Transição operacional só após validação completa

---

## Evolução API — Remoção
**Data:** Jun 2026

Removida do VPS por não estar em uso:
- Containers deletados
- Volumes removidos
- ~17GB liberados

---

## Builds Node — Sempre NVM
**Data:** Jun 2026

Antes de qualquer build Node.js na VPS, usar:
```bash
nvm use 20
```
Versão correta: v20.20.2

---

## CommercialRS — Sistema Separado
**Data:** Jun 2026

ComercialRS é completamente separado de:
- n8n
- Chatwoot
- Kanban

Cada um tem sua própria pasta, configurações e workflows.

---
*Última atualização: 2026-06-13*
