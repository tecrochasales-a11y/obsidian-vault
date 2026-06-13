# Cron e Automações — VPS

## Cron Jobs Ativos

### Obsidian Vault
```cron
*/5 * * * * /usr/local/bin/obsidian-sync.sh >> /var/log/obsidian-sync.log 2>&1
* * * * * cd /var/www/obsidian-vault && git pull origin main >> /var/log/obsidian-sync.log 2>&1
```
- Push: a cada 5 minutos
- Pull: a cada 1 minuto

### Task Alerts (ComercialRS)
Localizado em: `/var/www/comercialrs/data/task_alerts.py`
- Executa: a cada 5 minutos (`*/5 * * * *`)
- Alertas de reuniões atrasadas
- Lock: 9000s (2h30) anti-spam

## Systemd Services

### obsidian-site
```ini
[Unit]
Description=Obsidian Vault Web Server
After=network.target

[Service]
ExecStart=/usr/bin/node /var/www/obsidian-site/index.js
Restart=always
```
- Status: `systemctl status obsidian-site`
- Log: `journalctl -u obsidian-site -f`

## Python Scripts

### meta_webhook_vps_service.py
- Path: `/var/www/comercialrs/data/`
- Porta: 8083
- PID: verificado em execução
- Log: `/var/log/meta_webhook.log`

### task_alerts.py
- Path: `/var/www/comercialrs/data/`
- Anti-spam: lock 9000s
- Query: filtra `status = 'todo'`

## Edge Functions (Supabase Cloud)
Deploy via: `supabase functions deploy <nome>`

Funções principais:
- `dispatch-automation` — dispara automações
- `send-reminders` — envia lembretes WhatsApp
- `meta-webhook` — recebe webhooks Meta
- `crm-sales-sync` — sincroniza CRM

---
*Última atualização: 2026-06-13*
