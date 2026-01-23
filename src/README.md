# Mahailah — Educadora Financeira (Agente Didático)

Mahailah é um agente didático de educação financeira com interface em Streamlit que utiliza um modelo local via Ollama. Ele usa dados de exemplo do cliente para contextualizar o atendimento e responder de forma simples e direta.

---

## Estrutura do Projeto

```
.
├─ src/
│  ├─ app.py                 # Aplicação Streamlit
│  ├─ requirements.txt       # Dependências Python
│  └─ README.md              # Este guia
└─ data/
   ├─ transacoes.csv         # Transações recentes do cliente
   ├─ produtos_financeiros.json  # Catálogo de produtos para exemplos
   ├─ perfil_investidor.json # Perfil e objetivos do cliente
   └─ historico_atendimento.csv  # Registros de atendimentos anteriores
```

---

## Pré‑requisitos

- Python 3.10+ (recomendado)
- Ollama instalado e em execução (porta padrão 11434)
- Acesso local aos arquivos em `data/`

---

## Instalação

Você pode instalar as dependências diretamente via `pip`:

```bash
# No diretório do projeto
pip install -r src/requirements.txt
```

---

## Configuração do Ollama

1) Instale o Ollama (https://ollama.com)
2) Baixe um modelo leve (o projeto usa `gpt-oss` por padrão):

```bash
ollama pull gpt-oss
```

3) Inicie o serviço do Ollama:

```bash
ollama serve
```

Opcional: teste rápido do modelo

```bash
ollama run gpt-oss "Olá!"
```

---

## Como Executar

Com o Ollama rodando, inicie a interface Streamlit:

- Windows
```powershell
streamlit run .\src\app.py
```

- Linux/macOS
```bash
streamlit run src/app.py
```

Acesse o app no navegador (geralmente em `http://localhost:8501`).

---

## Como Funciona

- Arquivo principal: `src/app.py`
- O app carrega os dados de `data/` e monta um contexto do cliente.
- Um prompt de sistema (no próprio `app.py`) orienta o tom e as regras da Mahailah.
- As mensagens do usuário são enviadas ao Ollama (endpoint `http://localhost:11434/api/generate`) usando o modelo definido.

Principais variáveis de configuração (em `src/app.py`):

```python
OLLAMA_URL = "http://localhost:11434/api/generate"
MODELO = "gpt-oss"
```

Para trocar o modelo, altere `MODELO` e garanta que ele foi baixado com `ollama pull <nome-do-modelo>`.

---

## Dados de Exemplo (pasta `data/`)

- `perfil_investidor.json`: nome, idade, perfil, objetivo, patrimônio e reserva.
- `transacoes.csv`: tabela com transações financeiras recentes.
- `historico_atendimento.csv`: registros textuais de interações anteriores.
- `produtos_financeiros.json`: lista de produtos para exemplificação didática.

Você pode substituir esses arquivos pelos seus dados mantendo os mesmos nomes/formatos, ou ajustar os caminhos no `app.py`.

---

## Dicas e Personalização

- Tom e regras: edite o `SYSTEM_PROMPT` no `app.py` para refinar a personalidade e limites da Mahailah.
- Contexto: adicione/remova fontes em `data/` e atualize o carregamento no `app.py`.
- Desempenho: use modelos menores no Ollama para respostas mais rápidas.

---

## Problemas Comuns

- "Conexão recusada" ao chamar o modelo: verifique se o `ollama serve` está ativo e se a porta 11434 está livre.
- Modelo não encontrado: rode `ollama pull gpt-oss` (ou o modelo escolhido) antes de iniciar o app.
- Encoding de acentos: mantenha arquivos em UTF‑8. Se ver caracteres estranhos, ajuste o encoding do editor/terminal.

---

## Comandos Rápidos

```bash
# Instalar dependências
pip install -r src/requirements.txt

# Baixar modelo e iniciar serviço
ollama pull gpt-oss
ollama serve

# Executar a interface
streamlit run src/app.py
```

---

## Licença

Uso educativo/exemplificativo. Adapte conforme a política do seu projeto.
