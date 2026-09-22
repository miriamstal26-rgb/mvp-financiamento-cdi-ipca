# MVP - Pipeline de Dados para Análise Comparativa entre CDI, Selic e IPCA no Contexto de Financiamentos Imobiliários

## Objetivo

Analisar a evolução histórica dos indicadores CDI, Selic e IPCA utilizando uma arquitetura Lakehouse baseada no modelo Medalhão.

## Arquitetura

Landing → Bronze → Silver → Gold → Analysis

## Perguntas de Negócio

1. Como CDI, Selic e IPCA evoluíram ao longo dos últimos dez anos?
2. Qual teria sido o custo acumulado de um financiamento corrigido pelo CDI em comparação ao IPCA?
3. Qual indexador apresentou maior volatilidade?
4. Em quais períodos um financiamento indexado ao CDI apresentaria maior pressão financeira para o consumidor?

## Estrutura do Repositório

### `data/landing`

Contém os arquivos CSV originais obtidos a partir do Sistema Gerenciador de Séries Temporais (SGS) do Banco Central do Brasil. Esta pasta representa a Landing Zone do projeto, preservando os dados exatamente como foram extraídos da fonte.

Arquivos:

- `cdi_4389.csv`
- `selic_432.csv`
- `ipca_433.csv`

---

### `docs`

Contém a documentação técnica do projeto, incluindo a descrição da arquitetura utilizada e o catálogo dos dados produzidos ao longo do pipeline.

Arquivos:

- `arquitetura.md`
- `catalogo_dados.md`

---

### `notebooks`

Contém os notebooks desenvolvidos no Databricks e utilizados na implementação do pipeline de dados e das análises.

Arquivos:

- `01_ingest_bronze.ipynb`  
  Responsável pela ingestão dos arquivos CSV e construção da camada Bronze.

- `02_transform_silver.ipynb`  
  Responsável pela padronização, transformação e integração dos dados na camada Silver.

- `03_build_gold.ipynb`  
  Responsável pela construção das tabelas analíticas da camada Gold.

- `04_analysis.ipynb`  
  Responsável pelas análises exploratórias, visualizações e respostas às perguntas de negócio.

---


## Relatório Completo

O relatório completo está disponível no arquivo:

Relatorio_MVP_Financiamento_CDI_IPCA.pdf
