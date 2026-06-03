# 🤖 LAB-001: Agent CLI Single-Turn com Tool-use

[![Feito para a Residência Tecnológica do SiDi](https://img.shields.io/badge/SiDi-Residência%20Tecnológica-blue)](https://www.sidi.org.br/)

> 🧪 **Laboratório 1** da disciplina **Desenvolvimento de Software com IA Generativa**  
> 🏫 Residência Tecnológica do **SiDi** — Formação em IA Aplicada

---

## 📋 Sobre o Lab

Construir um **agente single-turn** que usa **function-calling** para resolver problemas com precisão — sem depender de alucinação do modelo.

**👨‍🏫 Professor:**  Nicksson Ckayo Arrais de Freitas

**🎯 Objetivos de Aprendizagem:**
- **M1-O4:** Implementar function-calling com schema JSON e loop de execução de ferramentas
- **M1-O5:** Comparar pure-prompt vs tool-use no mesmo problema

**📊 Classificação Bloom:** Apply  
**⏱️ Duração:** 90 min

---

## 🧩 O que o agente faz

| Tool | Descrição |
|---|---|
| 🧮 `calculator` | Avalia expressões aritméticas com `eval` seguro (whitelist de caracteres) |
| 📖 `lookup_doc` | Consulta corpus local de documentação técnica |

### 🔁 Fluxo de execução

```
Usuário → pergunta → LLM (+ schemas JSON) → tool_call? → executa função → resultado → LLM → resposta final
```

---

## 🛠️ Stack

- **Provider:** Groq API (`llama-3.3-70b-versatile`) — gratuita e estável para tool-use
- **SDK:** OpenAI Python SDK (endpoint compatível)
- **Validação:** Pydantic v2
- **Modelo:** Loop single-turn com `tool_choice="auto"`

---

## 🚀 Como usar

### 1️⃣ Configurar chave da API

1. Crie uma conta em [Groq Console](https://console.groq.com/keys)
2. Gere uma API key
3. Defina como variável de ambiente ou use o prompt interativo

### 2️⃣ Executar

```bash
pip install openai pydantic python-dotenv
```

Abra o notebook `01_agent_cli_tool_use.ipynb` e execute todas as células ▶️

### 3️⃣ Testar as queries

```python
run_agent("Quanto é 47 * 13 + 200?")       # → 811 🎯
run_agent("O que é retry?")                 # → definição 📖
run_agent("Calcule 25% de 480 e explique pydantic")  # → 120 + definição 🧮📖
```

---

## 📊 Comparativo: Pure-Prompt vs Tool-use

| Query | Pure-prompt | Tool-use | Recomendado |
|---|---|---|---|
| Aritmética com >2 dígitos | ⚠️ erro ocasional | ✅ sempre exato | **Tool-use** |
| Definição factual | 📚 depende do treinamento | 🎯 ancorado em fonte | **Tool-use** |
| Conversa aberta | 🗣️ natural | ⏳ overhead desnecessário | **Pure-prompt** |

---

## 🐛 Troubleshooting

| Problema | Causa | Solução |
|---|---|---|
| `tool_calls` = `None` | System prompt muito fraco | Reforçar instrução ou usar `tool_choice` forçado |
| `JSONDecodeError` | LLM gerou JSON inválido | Envolver `json.loads` em `try/except` |
| Loop infinito | Tool retornou erro e LLM repetiu | Padronizar resultados com `OK:` / `ERROR:` e instruir o modelo |

---

## 📁 Estrutura

```
📦 LLM-com-tools
 ┣ 📜 01_agent_cli_tool_use.ipynb   # Notebook principal do lab
 ┣ 📜 README.md                      # Você está aqui
```

---

## 👩‍💻 Autora

**Danielle Magalhães Ballester**  
Residência Tecnológica do SiDi — Turma de IA Aplicada  
🔗 [GitHub](https://github.com/daniballester-ai)

---

> ✨ *"Tool-use não é overkill — é a diferença entre um LLM que chuta e um agente que acerta."*
