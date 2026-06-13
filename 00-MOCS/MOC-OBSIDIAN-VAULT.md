# Obsidian Vault — Rocha Sales

## Acesso
- **Site:** https://obsidian.rochasalesseguros.com.br
- **GitHub:** https://github.com/tecrochasales-a11y/obsidian-vault
- **VPS Path:** `/var/www/obsidian-vault`

## Estrutura de Pastas
- [[00-MOCS/]] — Índice de sistemas e MOCs
- [[01-MEMORY/]] — Memórias do Hermes
- [[02-AGENTS/]] — Agentes e configurações
- [[03-SUPABASE/]] — Documentação do banco
- [[04-PROJECTS/]] — Projetos ativos
- [[05-WORKFLOWS/]] — Fluxos operacionais
- [[06-LOGS/]] — Histórico e logs
- [[07-DECISIONS/]] — Decisões arquiteturais
- [[08-KNOWLEDGE/]] — Base de conhecimento
- [[09-DOCUMENTATION/]] — Documentação
- [[10-BACKUPS/]] — Backups
- [[11-ARCHIVE/]] — Arquivo

## Tecnologias
- Servidor Node.js (porta 3099)
- Nginx (proxy reverso)
- SSL Let's Encrypt
- Git sync automático (push 5min / pull 1min)

## Começando
1. Acesse o site: https://obsidian.rochasalesseguros.com.br
2. Navegue pelas pastas
3. Arquivos .md são renderizados automaticamente

## Sync GitHub
```bash
/usr/local/bin/obsidian-sync.sh
```

## Serviço
- Nome: `obsidian-site`
- Status: `systemctl status obsidian-site`

---
*Criado: 2026-06-13*
