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

![mockup2](![mockup1](https://github.com/felipedidier/data_viz_notas_Enem/blob/main/image/mockup2.png?raw=true)

### Construção do Dashboard

O dashboard foi construído importando todos arquivos relacionais para a base de dados. A partir das correlações já testadas no ambiente SQL, foram construídos os indicadores e métricas.

#### Painel principal

![image](https://user-images.githubusercontent.com/47615255/158833171-d5ba416e-5606-470c-9ecf-c29485b075cf.png)

Como pode ser visto no painel principal abaixo, os principais indicadores de vendas estão listados acima. Essa visualização em formato de cartão foi escolhida pois permite uma captação mais rápida das informações. Nesses cartões, foram inseridas as informações de ticket médio, total de vendas e frete e a média de reviews score. Aplicando-se os filtros, fica fácil de visualizar que em 2018 houveram as melhores vendas e melhores reviews, comparando com 2016 e 2017. O que nos pode inferir que a satisfação do cliente está subindo proporcionalmente as vendas de ano em ano.

![image](https://user-images.githubusercontent.com/47615255/158836076-bbd5c32c-a39c-477f-aa73-165e65c8e1e4.png)

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


