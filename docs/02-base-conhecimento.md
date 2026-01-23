# Base de Conhecimento

> [!TIP]
> **Prompt Sugerido para esta etapa:**
> ```
> Preciso organizar minha base de conhecimento do meu agente [nome_seu_agente].
> Tenho esses arquivos de dados:
>   [listar os arquivos]
>
> Me ajude a:
>   (1) Entender o que cada arquivo contém.
>   (2) Descidir como usar cada um.
>   (3) Criar exemplos de contexto formatado para incluir no prompt.
>
> [cole o template 02-base-conhecimento.md]
> ```
---

## Dados Utilizados

| Arquivo | Formato | Como a Mahailah utilizará? |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores ou seja, dar continuidade de forma mais eficiente ao atendimento. |
| `perfil_investidor.json` | JSON | Personalizar as explicações sobre as dúvidas e necessidades de aprendizado do cliente. |
| `produtos_financeiros.json` | JSON | Conhecer produtos disponiveis para que eles possam ser ensinados o cliente. |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e usar essas informações de forma didática. |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

- O produto Fundo Imobiliario (FII) substituiu o Fundo Multimercado.
- Adicionado o Bitcoin como opções também como forma de investimento para fins de estudos.

Com essas alteraçoes pessoalmentee me sinto mais confiante em usar apenas produtos financeiros que eu conheço. Assim poderei validar melhor as respostas da Mahailah de forma mais assertiva.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Existe duas possibilidades, injetar os dados diretamente no prompt (crtl + c, crtl + v) ou carregar os arquivos via código, como no exemplo abaixo:

```python
import pandas as pd
import json

#CSV
historico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pdd.read_csv('data/trasacoes.csv')

#JSON
whith open('data/perfil_investidor.json', 'r', enconding='utf-8) as f:
   perfil = json.load(f)

whith open('data/produto_financeiro.json', 'r', enconding='utf-8) as f:
   perfil = json.load(f)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para simplificar, podemos simplesmente "injetar" os dados em nosso prompt, garantindo que o Agente tenha o melhor contexto possível. Lembrando que, em soluções mais robustas o ideal é que estas informações sejam carregadas dinâmicamente, para que possamos ganhar flexibilidade.

```text
DADOS DO CLIENTE E PERFIL: (data/perfil_investidor.json)
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

TRANSACOES DO CLENTE: (data/transacoes.csv)
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

HISTORICO DE ATENTIMENTO DO CLENTE: (data/historico_atendimento.csv)
data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

PRODUTOS DISPONIVEIS PARA ENSINO: (data/produtos_financeiros.json)
[
    {
    "nome": "Bitcoin (BTC)",
    "categoria": "Criptomoeda / Ativo Digital",
    "risco": "Alto",
    "rentabilidade": "Alta volatilidade, com potencial de alta no longo prazo e fortes oscilações no curto prazo",
    "aporte_minimo": 1,
    "indicado_para": "Investidores com perfil moderado a arrojado, que toleram alta volatilidade, possuem visão de longo prazo e buscam diversificação fora do sistema financeiro tradicional"
  },
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Imobiliario (FII)",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "Dividendo Yield (DY), costuma ficar entre 6% a 12% ao ano.",
    "aporte_minimo": 500.00,
    "indicado_para": "Perfil moderado que busca diversificação e renda recorrente mensal."
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo de contexto montado abaixo, se baseiam nos dados originais da base de conhecimento, mas os sintetizam deixando apenas as informações mais relevantes, otimizando assim o cosumo de tokens. Entretanto, vale lembrar que o mis importane de que economizar tokens, é ter todas as informações relevantes disponíveis em seu contexto.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Objetivo: Construir uma reserva de emergência.
- Reserva atual: R$ 10.000
- Meta: R$ 20.000

Resumo dos Gastos:
- Moradia: R$ 1.380
- Alimentação: R$ 570
- Transporte: R$ 295
- Saúde: R$ 188
- Lazer: R$ 55,90
- Total de saídas: R$ 2.488,90

Produtos dísponiveis para explicar:
- Bitcoin (BTC) - Risco Alto
- Tesouro Selic - Risco Baixo
- CDB Liquidez Diária - Risco Baixo
- LCI/LCA - Risco Baixo
- Fundo Imobiliario (FII) - Risco Médio
- "Fundo de Ações - Risco Alto
...
```
