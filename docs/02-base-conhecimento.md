# Base de Conhecimento

## Dados Utilizados

Arquivos da pasta `data`:

| Arquivo | Formato | Para que serve na Lia? |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores, ou seja, dar continuidade ao atendimento de forma mais personalizada |
| `perfil_investidor.json` | JSON | Personalizar as explicações sobre as dúvidas e necessidades de aprendizado do cliente |
| `produtos_financeiros.json` | JSON | Conhecer os produtos disóníveis para que eles possam ser ensinados ao cliente |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e usar essas informações de forma didática |

> [!TIP]
> Datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, que podem ser adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

O produto Fundo Imobiliário (FII) substituiu o Fundo Multimercado, uma vez que me sinto mais confiante em utilizar produtos financeiros que eu conheça, de forma que posso validar de forma mais assertiva as respostas da Lia.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.
Os JSON/CSV são carregados no início da sessão e incluídos no contexto do prompt

Existem 2 possibilidades:
  1. Injetar os dados diretamente no prompt
  2. Carregar os arquivos via código:

```python
import pandas as pd
import json

# CSVs

historico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pd.read_csv('data/transacoes.csv')

# JSONs

with open('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
    perfil = json.load(f)

with open('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
    produtos = json.load(f)

```
### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para simplificar, podemos "injetar" os dados em nosso prompt, garantindo que o Agente tenha o melhor contexto possível. Lembrando que, em soluções mais robustas, o ideal é que essas informações sejam carregadas dinamicamente para que possamos ganhar flexibilidade (ex: injetando com o código acima)

```text
DADOS  DO CLIENTE:

PREFIL DO CLIENTE:

TRANSAÇÕES DO CLIENTE:

HISTÓRICO DE ATENDIMENTO DO CLIENTE:

PRODUTOS DISPONÍVEIS PARA ENSINO:

```
---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo abaixo se baseia nos dados originais da base de conhecimento, mas os sintetiza, deixando apenas as informações mais relevantes, otimizando o consumo de tokens. Entretanto, vale lembrar que, mais importante que economizar tokens, é ter todas as informações relevantes disponíveis em seu contexto.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
