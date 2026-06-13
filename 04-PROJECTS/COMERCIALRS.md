# ComercialRS — CRM de Vendas

## Localização
- **Path:** `/var/www/comercialrs`
- **Porta:** 3010
- **Build:** TanStack Start (Vite)
- **Node:** v20.20.2 (usar `nvm use 20` antes de build)

## Stack
- TanStack Start (React + Vite)
- Supabase Cloud (`dauftiqcvgaydddoxqhh`)
- Meta WhatsApp Business API

## Meta WhatsApp (CUIDADO — NÃO CONFUNDIR COM CHATWOOT!)
- Webhook: `/var/www/comercialrs/data/meta_webhook_vps_service.py`
- Porta: 8083
- Recebe botões e textos → atualiza tarefas
- **NÃO** chama Chatwoot API

## Alertas
- `reunioes_atrasadas`: wrapper lock 9000s (2h30) + message_log anti-spam
- `task_alerts.py`: wrapper lock 9000s

## Build e Deploy
```bash
cd /var/www/comercialrs
nvm use 20
npm run build
# Output vai pra dist/
```

## Sistema Separado
- **NÃO** confundir com n8n, Chatwoot ou Kanban
- Cada um tem sua própria pasta e configuração

---
*Última atualização: 2026-06-13*
