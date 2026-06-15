# Hermes — Configuração e Setup

## O que é Hermes
Agente de IA CLI que funciona como assistente pessoal, rodando em VPS ou desktop.

## Instalação
```bash
# Clonar repositório
git clone https://github.com/rocha-hermes/hermes-agent.git
cd hermes-agent

# Criar ambiente virtual
python3 -m venv venv
source venv/bin/activate

# Instalar dependências
pip install -e .

# Configurar
hermes setup
```

## Configuração Inicial
Arquivo: `~/.hermes/config.yaml`
Ambiente: `~/.hermes/.env`

## Variáveis de Ambiente (.env)

### Discord
```env
DISCORD_BOT_TOKEN=REDACTED
DISCORD_ALLOWED_USERS=1466486464614371401
DISCORD_IGNORE_NO_MENTION=false
DISCORD_HOME_CHANNEL_ID=1492133095875543263
```

### Supabase
```env
SUPABASE_URL=https://dauftiqcvgaydddoxqhh.supabase.co
SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### OpenAI
```env
OPENAI_API_KEY=sk-...
```

## Gateway (Systemd)
```bash
hermes gateway start
hermes gateway stop
hermes gateway restart
hermes gateway status
```

O gateway é um serviço systemd do usuário: `hermes-gateway.service`

## Memória e Estado
- **State DB:** `~/.hermes/state.db` (SQLite, ~297MB, 91k+ mensagens)
- **Memory:** `~/.hermes/memories/MEMORY.md` + `USER.md`
- **Skills:** `~/.hermes/skills/` (52 skills, ~9.5MB)
- **Sessions:** `~/.hermes/sessions/`
- **Logs:** `~/.hermes/logs/`

## Skills do Sistema
- 52 skills organizadas por categoria
- Carregadas sob demanda
- Incluem: devops, Supabase, Chatwoot, n8n, VPS, etc.

## Discord Bot Info
- **Nome:** Rocha Sales Hermes
- **Bot ID:** 1485798532261609663
- **Server:** Rocha Sales (1492133095288209458)
- **Home Channel:** #geral (1492133095875543263)

## Fluxo de Mensagens
1. Mensagem chega no Discord
2. Gateway recebe e passa pro Agent
3. Agent processa (usa tools se necessário)
4. Resposta volta pelo Discord

## Comandos Slash
- `/help` — ajuda
- `/skills` — lista skills
- `/search` — busca em sessões
- `/model` — troca modelo
- etc.

## Migração para Obsidian
A partir de Jun/2026, memória operacional migrando para:
- **Vault:** `/var/www/obsidian-vault` (VPS)
- **Site:** https://obsidian.rochasalesseguros.com.br
- **GitHub:** https://github.com/tecrochasales-a11y/obsidian-vault

---
*Última atualização: 2026-06-13*
