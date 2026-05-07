# Data-Analysis_Ecommerce-Sales
O objetivo é transformar dados brutos de vendas financeiras de um ecommerce em uma ferramenta de tomada de decisão para economia, controle de gastos e identificar padrões recorrentes.

**Versão:** 1.0

**Autor:** Beatriz Almeida

**Ferramentas:**  Kaggle Dataset, Power Query, DAX e Power BI.

##
### 1. Visão Geral do Projeto
*“Este conjunto de dados contém registros históricos das transações de vendas de uma empresa de comércio eletrônico. Inclui informações como data do pedido, nome do produto, categoria, região, quantidade vendida, valor das vendas e lucro.”*

- **Fonte de Dados:** Dataset "ecommerce_sales.csv" extraído do Kaggle.
- **Período de Análise:** [Ex: Janeiro/2022 a Dezembro/2024].
##
### 2. Engenharia de Dados (ETL)
Nesta seção, foi detalhado o que foi feito no **Power Query Editor**. O foco aqui é a "limpeza" dos dados.

- **Reduzir linhas:** removi as linhas em branco.
- **Remoção de duplicatas:** removendo linhas idênticas por erro de exportação.
  
  <img width="1919" height="985" alt="image" src="https://github.com/user-attachments/assets/ac5456f9-94f9-4aff-876b-5197d55cd366" />

- **Normalização - colunas existentes:**
    - **Coluna “Order Date”:** alterei o formato para “short date” —> encurtando as informações.
    - **Coluna “Quantity”:** alterei o resumo de “Somar” para “Não resumir”.
    - **Coluna “Sales”:** alterei o formato para “moeda” - considerou em dólar, de acordo com a base de dados | adicionei a formatação “ponto” para separador de milhares.
    - **Coluna “Profit”:** alterei o formato para “moeda” - considerou em dólar, de acordo com a base de dados | adicionei a formatação “ponto” para separador de milhares.
  <img width="1919" height="1009" alt="image" src="https://github.com/user-attachments/assets/1f176acd-dd0a-4048-979e-df9d1ddef467" />

- **Normalização - criação de tabela:**
    - **Criação de tabela calendário** (Data completa, Ano, Mes, Mes Numero, AnoMes)
        - **Coluna Date:** alterei o formato para “short date” —> encurtando as informações.
        - **Coluna Mes:** Classificar por coluna → Mes Numero
##
### 3. Modelagem de Dados
O coração do Power BI, descritivo da estrutura do modelo.

- **Esquema de Dados:** **Star Schema** (Esquema Estrela).
- **Tabela Fato:** `Ecommerce_Sales` (Contém datas, produtos, região e valores).
- **Tabelas Dimensão:**
    - `dCalendario`: Essencial para inteligência de tempo.
- **Relacionamentos:**
    - `Date` [dCalendario] para `Order Date` [Ecommerce_Sales] —> Cardinalidade: Um para um (1:1)
##
### 4. Inteligência de Dados (Cálculos DAX)
Criei uma tabela para organização de medidas que serão criadas.

- `dMedidas` : armazenamento de cálculos

**4.1. Fórmulas Principais**

- **Total de Vendas:** `Total Sales` = SUM(Ecommerce_Sales[Sales])
- **Lucro Total: `**Total Profit` = SUM(Ecommerce_Sales[Profit])
- **Margem de Lucro:**  `Profit Margin` =  [Total Profit]/[Total Sales]
- **Vendas mês a mês (Passado):** `Sales Last Month` = CALCULATE([Total Sales],DATEADD('dCalendario'[Date], -1, MONTH)) | Ela olha para o período atual do seu filtro e "pula" para trás o quanto você definir.
- **Crescimento MoM (%) (Comparativo):**  `MoM Sales Growth % =` VAR VendasAtual = [Total Sales],  VAR VendasPassado = [Sales Last Month], VAR Variacao = [Total Sales] - [Sales Last Month]    RETURN DIVIDE(Variacao, VendasPassado, 0)
- **Vendas Acumuladas Ano:** `Sales YTD` = TOTALYTD([Total Sales],dCalendario[Date]) | Se você filtrar "Março/2024", essa medida vai somar Janeiro + Fevereiro + Março de 2024.
- **Acumulado Ano Anterior (Comparativo):**  `Sales LY YTD` = CALCULATE( [Sales YTD],SAMEPERIODLASTYEAR(dCalendario[Date]))   | Para saber se o seu projeto está indo bem, você precisa comparar o acumulado deste ano com o acumulado do mesmo período do ano passado.
- **Cálculo de Performance (%):** `YTD Growth %` = VAR Atual = [Sales YTD], VAR Passado = [Sales LY YTD]   RETURN    DIVIDE(Atual, Passado, 0)    | o quanto você cresceu no acumulado do ano em relação ao ano passado.
##
### 5. Design e Experiência do Usuário (UX/UI)
Para desenvolvimento do Layout e Design, me aprofundei na ferramenta Figma para proporcionar melhor experiência para o usuário. 
Há 3 interfaces: Capa, Performance de Vendas e Crescimento e Tendências (Em andamento).

#### Capa do Dashboard:

   <img width="767" height="451" alt="{6F74C687-B099-4AD8-BD5F-CE5CFA557815}" src="https://github.com/user-attachments/assets/0d31c84c-d897-4a2c-aee7-bfff46b66519" />

#### Performance de Vendas:

  <img width="767" height="451" alt="{A9DD305D-BF31-4472-9858-51B8AB89DBFB}" src="https://github.com/user-attachments/assets/15485970-842a-41d8-b40e-7e617660cf78" />

