# 📊 Dashboard Sport Store — Análise de Performance Comercial

Dashboard em Power BI desenvolvido para simular um cenário real de análise comercial de uma loja de material esportivo, com foco em **tratamento de dados sujos no Power Query** e construção de métricas de negócio em **DAX**.

## 🎯 Objetivo

Transformar uma base de vendas bruta e inconsistente — exportada de um sistema legado fictício — em um dashboard de 3 páginas capaz de responder às principais perguntas da diretoria comercial:

- Qual o faturamento total e como ele evolui mês a mês?
- Quais produtos mais vendem, em quantidade e em receita?
- Quem são os vendedores com melhor desempenho e qual seu peso no resultado total?
- Como o faturamento se distribui entre regiões e lojas?
- Existe sazonalidade por dia da semana ou mês?

## 🗂️ Sobre a base de dados

A base original (`vendas_loja_esportiva_sujo.csv`, ~440 linhas) foi gerada propositalmente com problemas comuns de dados de sistemas legados:

- Datas em **5 formatos diferentes** (`dd/mm/yyyy`, `yyyy-mm-dd`, `dd-Mon-yy`, etc.)
- Preços como **texto**, misturando vírgula e ponto decimal
- Nomes de produtos com **inconsistência de capitalização, espaços extras e sufixos de tamanho** (M, G) colados ao nome
- Nomes de vendedores e regiões com variações de formatação
- **Campos vazios** em Quantidade, Vendedor e Região
- **Linhas duplicadas** e linhas totalmente em branco

## 🛠️ Tratamento no Power Query

- Padronização de datas para o tipo Data nativo
- Conversão de preços para número decimal
- Limpeza de texto em Produto, Vendedor e Região
- Remoção do sufixo de tamanho do nome dos produtos
- Tratamento de valores ausentes (categorização como "Não informado")
- Remoção de duplicatas e linhas em branco

## 📐 Modelo de dados (DAX)

Principais medidas criadas:

Faturamento Total = SUMX(sport_store, sport_store[Quantidade] * sport_store[Preco_Unitario])

Qtd Total de Vendas = COUNTROWS(sport_store)

Qtd Total de Itens = SUM(sport_store[Quantidade])

Ticket Medio = DIVIDE([Faturamento Total], [Qtd Total de Vendas])

% do Total = 
DIVIDE(
    [Faturamento Total],
    CALCULATE([Faturamento Total], ALLSELECTED(sport_store[Vendedor]))
)

Colunas calculadas para análise de sazonalidade:

Dia Semana = FORMAT(sport_store[Data], "dddd")
Dia Semana Num = WEEKDAY(sport_store[Data], 2)

## 📑 Estrutura do dashboard

**Página 1 — Visão Geral**
Cards de Faturamento Total, Quantidade de Vendas, Ticket Médio e Quantidade de Itens; evolução mensal do faturamento; Top 10 produtos por receita.

**Página 2 — Vendedores e Região**
Tabela com Receita, Número de Vendas, Ticket Médio e % do Total por vendedor; distribuição de faturamento por região (gráfico de pizza e barras); slicers de Região, Loja, Vendedor e Mês.

**Página 3 — Sazonalidade e Detalhe de Produtos**
Faturamento por dia da semana; Top 10 produtos por quantidade vendida; tabela dinâmica completa de produtos com quantidade, receita e % do total.

## 🧰 Tecnologias

- Power BI Desktop
- Power Query (tratamento e modelagem de dados)
- DAX (medidas e colunas calculadas)

## 📁 Arquivos

- `projeto_loja_esportiva.pbix` — arquivo do dashboard
- `vendas_loja_esportiva_sujo.csv` — base de dados bruta utilizada

---

Projeto desenvolvido como prática de Power BI, com foco em qualidade de dados (Power Query) e modelagem analítica (DAX) para portfólio em Análise de Dados.
