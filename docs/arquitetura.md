# Arquitetura do Projeto

## Visão Geral

Este projeto adota a Arquitetura Medalhão (Medallion Architecture), amplamente utilizada em ambientes Lakehouse e sugerida nesta proposta de MVP, com o objetivo de organizar os dados em diferentes níveis de refinamento. Essa abordagem permite garantir rastreabilidade, governança, qualidade e reutilização dos dados ao longo do pipeline.

A arquitetura foi estruturada para separar claramente os dados brutos obtidos do Banco Central dos dados tratados e dos dados finais utilizados para responder às perguntas de negócio do projeto.

## Fluxo Geral dos Dados

```Banco Central (SGS)
       │
       ▼
┌─────────────────────────────┐
│ Landing (GitHub)            │
│ Arquivos CSV originais      │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ Bronze                      │
│ bronze_cdi                  │
│ bronze_selic                │
│ bronze_ipca                 │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ Silver                      │
│ dim_calendario               │
│ silver_indicadores_macro    │
│ silver_quality_report       │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ Gold                        │
│ gold_evolucao_indicadores   │
│ gold_risco_indexadores      │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ Análises e Visualizações    │
└─────────────────────────────┘
```

---

## Camada Landing

### Objetivo

Armazenar os arquivos exatamente como foram extraídos da fonte oficial.

### Justificativa

A camada Landing foi criada para preservar os dados originais obtidos do Sistema Gerenciador de Séries Temporais (SGS) do Banco Central. Essa separação garante rastreabilidade e permite reprocessar todo o pipeline sem necessidade de realizar uma nova coleta.

### Estrutura

- cdi_4389.csv
- selic_432.csv
- ipca_433.csv

---

## Camada Bronze

### Objetivo

Persistir os dados brutos dentro do ambiente Databricks.

### Justificativa

A camada Bronze mantém os dados o mais próximo possível da origem, adicionando apenas metadados operacionais necessários para controle da ingestão. Essa abordagem preserva a integridade dos dados originais e facilita auditorias futuras.

### Tabelas

- bronze_cdi
- bronze_selic
- bronze_ipca

---

## Camada Silver

### Objetivo

Padronizar, validar e consolidar as informações.

### Justificativa

Nesta etapa são realizadas as transformações necessárias para tornar os dados consistentes e comparáveis. Os arquivos exportados do SGS podem apresentar diferenças de estrutura, formatos e nomenclaturas que precisam ser corrigidas antes da análise.

Além disso, foi criada uma dimensão calendário para enriquecer as análises temporais e permitir agregações por mês, trimestre, semestre e ano.

### Tabelas

#### dim_calendario

Tabela de apoio contendo atributos temporais:

- data_referencia
- ano
- semestre
- trimestre
- mes
- nome_mes
- ano_mes

#### silver_indicadores_macro

Tabela consolidada contendo:

- data_referencia
- cdi_percentual
- selic_percentual
- ipca_percentual

#### silver_quality_report

Tabela destinada ao monitoramento da qualidade dos dados.

---

## Camada Gold

### Objetivo

Disponibilizar dados modelados para responder às perguntas de negócio.

### Justificativa

A camada Gold contém informações já refinadas e orientadas para análise. Seu foco é responder diretamente às questões relacionadas ao impacto dos indexadores CDI e IPCA em financiamentos imobiliários.

### Tabelas

#### gold_evolucao_indicadores

Utilizada para analisar a evolução histórica do CDI, Selic e IPCA.

#### gold_financiamento_indexado

Responsável por comparar a evolução de um saldo corrigido por CDI e por IPCA.

#### gold_risco_indexadores

Apresenta métricas de volatilidade e risco dos indexadores.

#### gold_simulacao_sac

Tabela complementar contendo uma simulação simplificada de financiamento imobiliário no sistema SAC.

---

## Benefícios da Arquitetura Escolhida

A utilização da Arquitetura Medalhão proporciona:

- Separação clara entre dados brutos e dados analíticos.
- Facilidade de rastreamento da origem dos dados.
- Maior governança e qualidade das informações.
- Reutilização das transformações desenvolvidas.
- Aderência às boas práticas recomendadas pela Databricks para ambientes Lakehouse.

Essa arquitetura permite demonstrar todas as etapas esperadas em um pipeline de Engenharia de Dados moderno, desde a captura dos dados até a geração de informações para tomada de decisão.
- Organização alinhada às boas práticas de Engenharia de Dados.
- Estrutura compatível com a arquitetura Lakehouse proposta pela Databricks.

Essa abordagem permite demonstrar todas as etapas de um pipeline de dados moderno, desde a ingestão dos dados até a disponibilização de informações para análise de negócio.
