# Projeto: Análise de Dados com PowerBI e SQLite
<img src="https://img.shields.io/badge/Status-Completed-brightgreen"/>

Projeto final do curso de Data Science da [Awari](https://awari.com.br/). O objetivo desse projeto é realizar os passos importantes para obtenção de uma análise de dados robusta e consistente do negócio.

Será criado um dashboard do **Comitê de Vendas da Olist para decisões do negócio**.

> Você pode acessar o dashboard [clicando aqui](AJUSTAR AQUI).

## Tópicos


* [Ambiente](#ambiente)
* [Dataset](#dataset)
* [Problemática](#problemática)
* [Descrição](#descrição)
* [Conclusão](#conclusão)

## Ambiente

Os dados do projeto foram analisados em SQL no DBrowser e o dashboard foi implementado em Power BI.

## Dataset

O projeto utilizará o [dataset do e-commerce Olist](https://www.kaggle.com/olistbr/brazilian-ecommerce). Esse dataset foi escolhido devido a quantidade de arquivos distintos e relações possíveis que podem ser montadas, conseguindo explorar bem as habilidades em SQL e no Power BI.

Esse dataset possui arquivos com informações de sobre as vendas na plataforma, como a lista de produtos, a identificação de vendedores e consumidores, a data de compra e entrega, as formas de pagamento, etc. Todas essas informações podem ser acessadas com mais detalhe no [link do dataset](https://www.kaggle.com/olistbr/brazilian-ecommerce).

## Problemática

Algumas perguntas do negócio necessitam ser respondidas ao Comitê de Vendas da Olist para que se possa gerar um planejamento estratégico com a finalidade de turbinar as vendas e melhorar o review.
As perguntas são:
- Qual ano teve a melhor performance de vendas e também de reviews?
- Quais estados possuem clientes dispostos a gastar mais?
- Qual a categoria que apresenta melhor oportunidade de ticket médio?
- Quais os estados que possuem um Ticket Médio alto?
- Quais estados concentram a maior quantidade de clientes?
- Há alguma correlação entre pedidos atrasados e o review score?
- Qual a percentagem de pedidos atrasados?
- Qual a média de tempo previsto e realizado dos pedidos?
- Qual a venda e frete total por categoria?
- Quais as quantidades mensais de venda e produto de cada categoria?
- Qual a quantidade de pedidos por nota de review e quais estados estão com os piores reviews?
- Há alguma correlação entre a intensidade de vendas e o review?

## Descrição

### Queries

O primeiro passo para compreender que o dataset consegue fornecer todas as respostas para as perguntas da Problemática é investigarmos e criarmos queries no SQL para validar se os dados conseguem de fato ser gerados.


### Dashboard

Finalizadas as tratativas no DBrowser, evoluimos para a construção do dashboard.

#### Mockup

Tendo em vista todos os dados que necessitam povoar o dashboard, é importante estruturar o modelo para que fique visualmente agradável e direto para os usuários finais. Nesse momento, a construção do mockup se torna fundamental para atender essa necessidade.

O projeto foi dividido em 3 páginas do dashboard:
- Painel principal;
- Painel de produtos;
- Painel de reviews.

Para o painel principal, foi construído o mockup abaixo. Essa estrutura foi pensada para destacar os indicadores principais do dashboard e também para permitir uma navegação dinâmica pelas métricas.

![mockup1](https://github.com/felipedidier/data_viz_notas_Enem/blob/main/image/mockup1.png?raw=true)

Nos outros dois painéis a mesma estrutura base do mockup será aplicada, tendo aplicações de gráficos e tabelas distintas.

![image](https://user-images.githubusercontent.com/47615255/158843062-32262664-e515-4d21-be80-5f2d148b1d3e.png)

### Construção do Dashboard

O dashboard foi construído importando todos arquivos relacionais para a base de dados. A partir das correlações já testadas no ambiente SQL, foram construídos os indicadores e métricas.

#### Painel principal

![image](https://user-images.githubusercontent.com/47615255/158833171-d5ba416e-5606-470c-9ecf-c29485b075cf.png)

Como pode ser visto no painel principal abaixo, os principais indicadores de vendas estão listados acima. Essa visualização em formato de cartão foi escolhida pois permite uma captação mais rápida das informações. Nesses cartões, foram inseridas as informações de ticket médio, total de vendas e frete e a média de reviews score. Aplicando-se os filtros, fica fácil de visualizar que em 2018 houveram as melhores vendas e melhores reviews, comparando com 2016 e 2017. O que nos pode inferir que a satisfação do cliente está subindo proporcionalmente as vendas de ano em ano.

![image](https://user-images.githubusercontent.com/47615255/158836975-9043bee7-c114-40a4-abe3-21a27f175c24.png)

Para se ter uma visualização mais dinâmica do ticket médio por estado, utilizou-se a visualização em mapa de formas, que permite analisar a partir da saturação de cores qual o estado com maior ticket médio e com isso saber qual o estado que possui os clientes dispostos a gastar mais por compra. Analisando o mapa, consegue-se concluir que Bahia e Pernambuco possuem os maiores tickets médios do país.

![image](https://user-images.githubusercontent.com/47615255/158836246-71a69ae9-f4bd-49da-9292-439c4f2b257d.png)

O Ticket médio por categoria permite coletar a informação de qual a categoria de produtos que os clientes estão gastando mais em todo o território brasileiro. Dessa forma, as categorias com maiores tickets médios tornam-se uma oportunidade interessante para futuras decisões de marketing.

![image](https://user-images.githubusercontent.com/47615255/158836173-9b09c123-7450-4d17-8ce9-10452d015644.png)

Além do Ticket médio é interessante analisar a venda total por categoria. Dessa forma, o gráfico "Venda Total por categoria" consegue nos retornar as 5 categorias com maior arrecadação em vendas, seja no país todo ou aplicando o filtro por estado.

![image](https://user-images.githubusercontent.com/47615255/158836311-0b5694b5-025e-47cb-9b06-38fb6dafec20.png)

No gráfico de rosca, que informa o Status de Entrega, é possível constatar que, no Brasil, o percentual de produtos entregues fora do prazo representam 8%, que representam um review score médio de 2,51. Ao lado do gráfico de rosca, é possível analisar os cartões de tempo estimado para ocorrer a entrega e tempo realizado. Quando analisados os pedidos entregues no prazo, o tempo estimado é de 24 dias e o tempo realizado, 11. Ao analisar os pedidos com prazo perdido, o valor de tempo realizado sobe para 32 dias, enquanto a previsão era de 22.

![image](https://user-images.githubusercontent.com/47615255/158836368-6e28d553-ac4d-4daf-97b6-1161762cb253.png)

Para completar a composição das informações contidas no painel principal, o gráfico de Pareto possibilita ter uma análise da quantidade de clientes por estado, onde pode-se concluir que 80% dos clientes estão concentrados nos estados de SP, RJ, MG, BA, RS e SC.

![image](https://user-images.githubusercontent.com/47615255/158836567-1a7a9ddf-abf0-4fc6-9539-859cbf053e87.png)

#### Painel de produtos

![image](https://user-images.githubusercontent.com/47615255/158833367-634f5bb1-db9e-4442-867c-f8ef77247e2d.png)

No painel de produtos é possível ver a lista de todas as categorias e a participação de cada uma no quesito vendas.

![image](https://user-images.githubusercontent.com/47615255/158837295-77778948-f8ad-4f38-a7ea-d5fda6f58baf.png)

Indo ainda mais no detalhe, é possível visualizar a venda da categoria ao longo do ano e identificar os meses que se tem maior venda. O dashboard lista inicialmente as duas categorias com maior participação de vendas, mas é possível selecionar a categoria desejada.

![image](https://user-images.githubusercontent.com/47615255/158837568-7a75ee3e-dc57-473f-ac36-68663fa38045.png)

Somando a análise acima, também é possível analisar a quantidade de produtos vendidos por categoria ao longo dos meses. Essa análise é interessante para identificar se o quantitativo total de vendas da categoria se dá pelo elevado preço dos produtos ou pela quantidade vendida.

![image](https://user-images.githubusercontent.com/47615255/158838348-a2aba92d-5a31-4097-9ef6-4bf50c6289eb.png)

#### Painel de reviews

![image](https://user-images.githubusercontent.com/47615255/158842860-dc0862b3-78a3-44e5-8666-057ba8c5af35.png)

Na análise de reviews, conseguimos verificar todos os questionamentos gerados pelo review score.
A partir da análise do mapa, é possível visualizar, pela grandeza dos circulos, quais os estados que tiveram maiores e menores notas no review. Por exemplo o Amapá é o estado que apresenta o menor score.

![image](https://user-images.githubusercontent.com/47615255/158838913-244ba08e-b4ec-4ba1-8fb0-ca6cae1edfb8.png)

Para as informações abaixo, foram agrupados por review score a quantidade de pedidos que foram avaliados. Dessa forma, é possível saber quantas avaliações foram destinadas para cada nota. Sabe-se também que 23% das avaliações são menores que 4.

![image](https://user-images.githubusercontent.com/47615255/158839961-3a2f63bf-3eb6-406c-bda5-b8545027cf1d.png)

Por fim, foi realizada uma análise exploratória para identificar se havia correlação entre o aumento das vendas e o review negativo. Esse indicador poderia sinalizar a falta de estrutura de atendimento ou entregas quando a demanda aumentava. Aparentemente, pelo gráfico, nenhuma conclusão pode ser tirada, apesar de 2017 chamar a atenção que ao aumentar as vendas no final do ano, o review score diminuiu.

![image](https://user-images.githubusercontent.com/47615255/158840678-1f182c6a-d081-40fa-91df-d1952415e4f2.png)

## Conclusão

O dashboard é bastante intuitivo para conseguir guiar o negócio em futuras estratégias de marketing, melhoria e logística, uma vez que conseguem ser extraidas informações interessantes. Algumas melhorias visíveis no dashboard são na profundidade de algumas análises, como por exemplo dos reviews. É muito oportuno abranger a análise dos reviews e conseguir analisar diretamente os textos dos comentários. Da mesma forma, é interessante também dar uma aprofundada maior na análise das categorias, visto que há muita coisa que pode ser explorada.
