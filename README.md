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

O primeiro passo para compreender que o dataset consegue fornecer é investigar e criar queries no SQL para validar se os dados conseguem de fato ser gerados. A partir desse princípio, foram exploradas várias queries que geraram dados interessantes pelo DBrowser.

- Venda por categoria (rankeando as maiores)

Query:
```sql
--1 Venda por categoria (rankeando as maiores)
WITH venda AS (
SELECT
pr.product_category_name as categoria,
SUM(p.payment_value) as vendas
FROM olist_order_payments_dataset p
JOIN olist_order_items_dataset i ON p.order_id = i.order_id
JOIN olist_products_dataset pr ON i.product_id = pr.product_id
GROUP BY 1
ORDER BY 2 DESC)
SELECT
v.categoria,
ROUND(v.vendas,2) as "vendas",
ROUND(SUM(v.vendas) OVER (PARTITION BY 1 ORDER BY v.vendas DESC), 2) as "vendas_acum"
FROM venda as v
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158846707-3c5fa771-7e22-46fd-8b87-0eecff93c7a9.png)


- Venda por produto (rankeando as maiores)

Query:
```sql
--2 Venda por produto (rankeando as maiores)
WITH venda_p AS (
SELECT
i.product_id as produto,
SUM(p.payment_value) as vendas
FROM olist_order_payments_dataset p
JOIN olist_order_items_dataset i ON p.order_id = i.order_id
GROUP BY 1
ORDER BY 2 DESC)
SELECT
v.produto,
ROUND(v.vendas,2) as "vendas",
ROUND(SUM(v.vendas) OVER (PARTITION BY 1 ORDER BY v.vendas DESC), 2) as "vendas_acum"
FROM venda_p as v
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158846838-31ab27dc-6ffb-4d0f-9a98-5a2ac834b048.png)

- Novos consumidores por mês/dia geral/estados

Query:
```sql
--3 Novos consumidores por mês/dia geral/estados
WITH datacomp AS (
SELECT
DISTINCT c.customer_unique_id "clientes",
c.customer_state "estado",
julianday(o.order_purchase_timestamp) "data_Compra",
o.order_purchase_timestamp "data"
FROM
olist_customers_dataset c
LEFT JOIN
olist_orders_dataset o ON c.customer_id = o.customer_id
LEFT JOIN
olist_order_payments_dataset p ON o.order_id = p.order_id
GROUP BY 1
ORDER BY 3 ASC
)
SELECT
d.clientes,
d.estado,
CASE
WHEN SUBSTR(d.data, 0, 5) == "2016" THEN 2016
WHEN SUBSTR(d.data, 0, 5) == "2017" THEN 2017
WHEN SUBSTR(d.data, 0, 5) == "2018" THEN 2018
ELSE NULL
END AS ano,
CASE
WHEN SUBSTR(d.data, 6, 2) == "01" THEN 1
WHEN SUBSTR(d.data, 6, 2) == "02" THEN 2
WHEN SUBSTR(d.data, 6, 2) == "03" THEN 3
WHEN SUBSTR(d.data, 6, 2) == "04" THEN 4
WHEN SUBSTR(d.data, 6, 2) == "05" THEN 5
WHEN SUBSTR(d.data, 6, 2) == "06" THEN 6
WHEN SUBSTR(d.data, 6, 2) == "07" THEN 7
WHEN SUBSTR(d.data, 6, 2) == "08" THEN 8
WHEN SUBSTR(d.data, 6, 2) == "09" THEN 9
WHEN SUBSTR(d.data, 6, 2) == "10" THEN 10
WHEN SUBSTR(d.data, 6, 2) == "11" THEN 11
WHEN SUBSTR(d.data, 6, 2) == "12" THEN 12
ELSE NULL
END AS mes
FROM datacomp d
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158846887-48adeb03-073f-461c-a967-9210d673c453.png)

- Vendas totais

Query:
```sql
--Vendas totais
SELECT
p.payment_type "forma_pagamento",
ROUND(SUM(p.payment_value),2) "valor"
FROM olist_order_payments_dataset p
GROUP BY 1
ORDER BY 2 DESC
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158846919-c8553486-b157-4e69-b74a-08587cb64f88.png)

- Análise dos dados de order_status

Query:
```sql
--Análise dos dados de order_status
WITH status AS(
SELECT
o.order_status "status",
o.order_purchase_timestamp "data",
CAST(SUBSTR(o.order_purchase_timestamp, 0, 5) AS INTEGER) AS "ano",
CAST(SUBSTR(o.order_purchase_timestamp, 6, 2) AS INTEGER) AS "mes"
FROM olist_orders_dataset o
) SELECT
s.status,
s.ano,
s.mes,
COUNT(*) as "quantidade"
FROM status s
GROUP BY 1, 2, 3
ORDER BY 2, 3, 4 DESC
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158846947-5daae1ce-ae00-4421-8ced-511371561c38.png)

- Correlação de venda total por quantidade total de produto

Query:
```sql
--Correlação de venda total por quantidade total de produto
SELECT
i.product_id,
COUNT(*),
ROUND(SUM(i.price),2)
FROM olist_order_items_dataset i
GROUP BY 1
order by 2 DESC
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158846971-21bc5882-aef2-4c45-9c95-af6464e38c07.png)

- Tempo médio da entrega móvel

Query:
```sql
-- Tempo médio da entrega móvel
WITH data as (
SELECT
  o.order_id AS "pedido",
  CAST(SUBSTR(o.order_purchase_timestamp, 0, 5) AS INTEGER) as "ano",
  CAST(SUBSTR(o.order_purchase_timestamp, 6, 2) AS INTEGER) as "mes",
  CAST(SUBSTR(o.order_purchase_timestamp, 9, 2) AS INTEGER) as "dia",
  JULIANDAY(o.order_delivered_carrier_date) - JULIANDAY(o.order_purchase_timestamp) AS "temp_post"
FROM olist_orders_dataset AS o
WHERE o.order_delivered_customer_date IS NOT NULL --filtro para retirar todos os pedidos que ainda não tiveram entrega realizada
GROUP BY 1
ORDER BY 2, 3, 4
) SELECT
d.ano,
d.mes,
d.dia,
ROUND(AVG(d.temp_post),2) as temp_post
FROM data d
GROUP BY 1, 2, 3
```
Saída:

![image](https://user-images.githubusercontent.com/47615255/158847001-32a6eae0-4e94-4ec5-bde9-907273b8d61e.png)


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
