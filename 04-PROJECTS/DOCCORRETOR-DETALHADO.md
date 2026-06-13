# DocCorretor — Migração Google Drive (Jun 2026)

## Resumo
Migração de 138 documentos do Google Drive para o DocCorretor.

**Status: ✅ COMPLETA**

## Dados da Migração
| Métrica | Valor |
|---|---|
| Documentos migrados | 138 |
| Arquivos no storage | 138 |
| Todos matched | ✅ |
| Clientes | 12 |

## Clientes Migrados
| Cliente | Qtd docs |
|---|---|
| Alessandra | 12 |
| Amil | 9 |
| Andreza | 8 |
| Ariani | 13 |
| Bruna Will | 13 |
| Bruno Felipe | 19 |
| Cassius | 23 |
| Daniel | 22 |
| Dayane | 10 |
| Augusto | 9 |

## Detalhes Técnicos
- **App:** `/var/www/documentos2/app/`
- **Porta:** 3010
- **Drive Folder ID:** `1zVhApvpFM937qxsvsz37WCcUD-7f2zUP`
- **Storage path:** `/uploads/{user_id}/{client_id}/{stored_name}`
- **Títulos:** limpos (sem timestamps)
- **Permissões:** `o+r` (leitura pública)

## URL
https://documentos.rochasalesseguros.com.br

## Stack
- TanStack Start (React + Vite)
- Supabase Cloud (`dauftiqcvgaydddoxqhh`)
- Google Drive (fonte original)
- cloudflared (tunnel porta 3010)

## Fluxo "Link para Corretor" (NÃO ALTERAR!)
1. Admin clica → token + pasta criada com `client_id=null`
2. Link gerado com `folder_id`
3. Externo preenche formulário
4. API atualiza pasta com `client_id`

## Build e Deploy
```bash
cd /var/www/documentos2/app
PATH="/root/.nvm/versions/node/v20.20.2/bin:$PATH" nvm use 20
node ./node_modules/vite/bin/vite.js build
# Copiar dist/ → documentos2/app/dist/
sudo systemctl restart documentos
```

## Histórico da Migração
- Início: Jun 2026
- Drive: pasta pública com ~2500+ arquivos
- Desafios resolvidos:
  - gdown parou de funcionar (permissões Drive)
  - Tabelas `clients` e `client_folders` estavam vazias
  - 138 documentos inseridos com sucesso
  - 138 arquivos no storage — 100% match

---
*Última atualização: 2026-06-13*
