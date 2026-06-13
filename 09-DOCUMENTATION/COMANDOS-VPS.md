# VPS — Comandos Essenciais

## SSH
```bash
sshpass -p 'Marcia19671951@' ssh -o StrictHostKeyChecking=no root@31.97.243.106
```

## Node/NVM
```bash
source ~/.nvm/nvm.sh && nvm use 20
```

## Docker
```bash
docker ps -a
docker stop <container>
docker rm <container>
docker volume ls
docker volume rm <volume>
```

## Nginx
```bash
nginx -t
nginx -s reload
systemctl restart nginx
```

## Systemd
```bash
systemctl status <service>
systemctl restart <service>
systemctl enable <service>
```

## Certbot
```bash
certbot --nginx -d obsidian.rochasalesseguros.com.br
certbot renew --dry-run
```

## Git (Vault)
```bash
cd /var/www/obsidian-vault
git add -A && git commit -m "mensagem"
git push origin main
/usr/local/bin/obsidian-sync.sh
```

## Logs
```bash
tail -f /var/log/obsidian-sync.log
journalctl -u obsidian-site -f
cat /var/log/nginx/error.log
```

## Ports
```bash
ss -tlnp | grep LISTEN
lsof -i :3099
```

---
*Última atualização: 2026-06-13*
