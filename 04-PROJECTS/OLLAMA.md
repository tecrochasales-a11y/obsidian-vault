# Ollama — LLMs Locais

## Status (Jun 2026)
- ✅ Rodando no VPS
- Container: `ollama-d0xb_ollama` (ativo)
- Porta: **11434**
- Rede: `ollama-d0xb_default`

## Modelos
Ollama serve modelos de LLM localmente, sem necessidade de API externa.

## Uso no Ecossistema
- Não conectado ativamente ao ComercialRS/DocCorretor
-Disponível para uso futuro
- CPU/GPU do VPS usada para inferência

## Volumes Removidos (Jun 2026)
- ❌ `ollama-nef5_ollama` (15GB) — era de instância antiga
- ❌ `ollama_data` (1.8GB) — dados órfãos

## Comandos Úteis
```bash
# Listar modelos
curl http://localhost:11434/api/tags

# Gerar resposta
curl http://localhost:11434/api/generate -d '{
  "model": "llama2",
  "prompt": "Hello"
}'

# Chat
curl http://localhost:11434/api/chat -d '{
  "model": "llama2",
  "messages": [{"role": "user", "content": "Hello"}]
}'
```

## Volumes Ativos
Volume ativo do container atual:
`ollama-d0xb_ollama`

## Recursos
- Sem custo de API
- Processa localmente
- Privacidade total
- Pode usar modelos como Llama2, Mistral, etc.

---
*Última atualização: 2026-06-13*
