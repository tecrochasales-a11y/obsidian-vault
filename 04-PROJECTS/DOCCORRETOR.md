# DocCorretor — Gerenciamento de Documentos

## Localização
- **App:** `/var/www/documentos2/app/`
- **Porta:** 3010
- **Build:** TanStack Start (Vite)
- **Node:** v20.20.2

## Stack
- TanStack Start (React + Vite)
- Supabase Cloud (`dauftiqcvgaydddoxqhh`) — mesmo projeto do ComercialRS
- Google Drive (migration concluída Jun 2026)
- cloudflared (porta 3010)

## Migração Google Drive (Jun 2026)
- **138 documentos** migrados
- **138 arquivos** no storage (100% matched)
- **12 clientes:** Alessandra(12), Amil(9), Andreza(8), Ariani(13), Bruna will(13), Bruno Felipe(19), Cassius(23), Daniel(22), Dayane(10), Augusto(9)
- Títulos limpos (sem timestamps)
- Permissões corrigidas (o+r)

## Fluxo "Link para Corretor" (NÃO ALTERAR!)
1. Admin clica → token + pasta criada com `client_id=null`
2. Link gerado com `folder_id`
3. Externo preenche formulário
4. API atualiza pasta com `client_id`

**Este fluxo é crítico — não modificar sem autorização do Tecrocha.**

## Build e Deploy
```bash
cd /var/www/documentos2/app
PATH="/root/.nvm/versions/node/v20.20.2/bin:$PATH" nvm use 20
node ./node_modules/vite/bin/vite.js build
# Copiar dist/ → documentos2/app/dist/
sudo systemctl restart documentos
```

## Estado
Estável desde Maio 2026.

---
*Última atualização: 2026-06-13*
