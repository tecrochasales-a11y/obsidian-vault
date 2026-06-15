# Memória Persistente — Hermes

## Identidade do Usuário
- **Nome:** Tecrocha Sales (Rocha Sales)
- **Perfil:** Não-técnico, prefere português simples
- **Canal:** Discord (DM)
- **Última atividade:** Junho 2026
- **Preferência:** Sempre pergunta "vai quebrar?" antes de aprovar mudanças em produção

## Sistemas Principais
- **Supabase Cloud:** `dauftiqcvgaydddoxqhh` — usado por ComercialRS e DocCorretor
- **VPS:** IP 31.97.243.106 — root / senha: `Marcia19671951@`
- **ComercialRS:** CRM de vendas — `/var/www/comercialrs`
- **DocCorretor:** Gerenciamento de documentos — `/var/www/documentos2/app/`

## ⚠️ Regras Críticas

### Nunca confundir
- Meta WhatsApp do **ComercialRS** ≠ Meta WhatsApp do **Chatwoot**
- ComercialRS: webhook em `/var/www/comercialrs/data/meta_webhook_vps_service.py` (porta 8083)
- Chatwoot: configuração separada, outra pasta, **NÃO** conflitar

### Antes de mexer em código
1. Verificar o que estava funcionando ontem
2. Confirmar que não vai quebrar nada
3. Perguntar "vai quebrar?" antes de aprovar

## Dados de Acesso
- **Login Painel CRM:** contato@rochasalesseguros.com.br / 221109
- **Supabase:** projeto `dauftiqcvgaydddoxqhh`
- **VPS:** 31.97.243.106 / root / Marcia19671951@

## Projetos Ativos (Jun 2026)
- **ComercialRS** — CRM de vendas (TanStack Start + Supabase)
- **DocCorretor** — Gerenciamento de documentos (TanStack Start + Supabase + Google Drive)
- **Chatwoot** — Atendimento (VPS)
- **n8n** — Automações (VPS)
- **Obsidian Vault** — Cérebro central (VPS) — MIGRADO EM JUN/2026

## Estrutura de Identidades (Supabase)
```
auth.users.id (UUID)
  → profiles.user_id → profiles.id (mesmo valor)
  → team_members.id (diferente!)
Tasks: created_by = profile.id, responsible_id = team_members.id
```

## Migração Obsidian (Jun 2026)
- Hermes migrando memória operacional para Obsidian Vault
- Vault: `/var/www/obsidian-vault` no VPS
- Site: https://obsidian.rochasalesseguros.com.br
- GitHub: https://github.com/tecrochasales-a11y/obsidian-vault
- Sync automático: push a cada 5 min, pull a cada 1 min

## DocCorretor — Drive Migration (Jun 2026)
- **138 docs** migrados, **138 storage files** (100% matched)
- **12 clientes:** Alessandra(12), Amil(9), Andreza(8), Ariani(13), Bruna will(13), Bruno Felipe(19), Cassius(23), Daniel(22), Dayane(10), Augusto(9)
- Títulos limpos (sem timestamps)
- Permissões corrigidas (o+r)
- cloudflared na porta 3010

## Spam Fix (Jun 2026)
- `task_alerts.py`: wrapper lock 9000s (2h30)
- `reunioes_atrasadas`: wrapper lock 9000s + message_log check anti-spam

## Builds Node — Sempre NVM
```bash
nvm use 20
```
Versão correta: v20.20.2

## Como o Tecrocha avalia mudanças
Sempre pergunta: "vai quebrar [sistema]?" ou "vai continuar funcionando?"
Precisa de confirmação explícita de que os fluxos mantêm funcionamento antes de aprovar.

## Canal Discord (Jun 2026)
- 100% ComercialRS — scripts rodam sozinhos
- User prefere opções mais simples/seguras sobre abordagens complexas
- Currently migrando Hermes brain para Obsidian vault

---
*Última atualização: 2026-06-15*
