# Base de Conhecimento

## Dados Utilizados


| Arquivo | Formato | Para que serve para Clara? |
|---------|---------|---------------------|
| `historico_chat.json` | JSON | O histórico de chat serve para dar memória ao agente, mantendo contexto das conversas, evitando contradições e permitindo respostas mais personalizadas. |
| `perfil_usuario.json` | JSON | O perfil do usuário permite personalizar as recomendações com base na renda, idade, profissão e nível de risco. |
| `historico_alertas.json` | JSON | O histórico de alertas registra todos os avisos financeiros já enviados ao usuário (ex: gasto acima do limite, comprometimento maior que 60%). |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

- transacoes.csv

Estruturou as transações com os campos: data, descricao, categoria, valor, tipo.
Padronizou entradas e saídas para permitir cálculo de comprometimento de renda.
Organizou os dados para leitura automatizada pelo agente.

- perfil_usuario.json

Definiu os campos: nome, idade, profissao, renda_mensal, nivel_sensibilidade.
Criou base para personalização das respostas e definição de limites financeiros.

- historico_alertas.json

Criou estrutura para registrar alertas gerados com base nas transações. 
Permitiu rastrear percentual de comprometimento da renda e excesso de gastos.

- historico_chat.json

Estruturou com: pergunta_usuario, resposta_agente, timestamp.
Adicionou memória contextual para manter coerência nas conversas.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

No sistema proposto, o carregamento de dados ocorre no Orquestrador desenvolvido em Python, que é acionado sempre que o usuário envia uma mensagem pela interface (Streamlit). Nesse momento, o orquestrador realiza a leitura dos arquivos estruturados que substituem a integração direta com instituições financeiras. As transações são carregadas a partir de arquivos CSV, permitindo o processamento de entradas, saídas e o cálculo do comprometimento da renda. Já as informações de perfil do usuário, histórico de conversas e histórico de alertas são lidas a partir de arquivos JSON, possibilitando personalização, manutenção de contexto e rastreabilidade das interações.

Após o carregamento, os dados são enviados ao módulo de cálculo financeiro, que gera indicadores e possíveis alertas. Esses resultados são então encaminhados ao modelo de linguagem para a geração da resposta ao usuário, passando antes por uma camada de segurança e validação. Ao final do fluxo, novos alertas e interações são gravados novamente nos arquivos JSON, garantindo persistência e atualização contínua das informações utilizadas pelo agente.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Após o carregamento e os cálculos, os dados (perfil, indicadores financeiros e histórico relevante) são inseridos como contexto interno no prompt do LLM.
Eles orientam o modelo a gerar respostas personalizadas, adequadas ao perfil do usuário, à situação financeira atual e às regras de segurança do sistema.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

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
