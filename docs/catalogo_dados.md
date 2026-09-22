# Catálogo de Dados

## Visão Geral

Este documento apresenta as tabelas utilizadas ao longo do pipeline de dados construído para análise dos indicadores econômicos CDI, Selic e IPCA.

A solução foi organizada seguindo a Arquitetura Medalhão (Landing, Bronze, Silver e Gold), permitindo rastreabilidade, governança e consumo analítico dos dados.

---

# Camada Landing

## cdi_4389.csv

**Origem:** Banco Central do Brasil (SGS 4389)

**Descrição:** Série histórica do CDI.

| Campo | Tipo | Descrição |
|---------|---------|---------|
| data | String | Data da observação |
| valor | String | Valor do CDI |

---

## selic_432.csv

**Origem:** Banco Central do Brasil (SGS 432)

**Descrição:** Série histórica da Taxa Selic.

| Campo | Tipo | Descrição |
|---------|---------|---------|
| data | String | Data da observação |
| valor | String | Valor da Selic |

---

## ipca_433.csv

**Origem:** Banco Central do Brasil (SGS 433)

**Descrição:** Série histórica do IPCA.

| Campo | Tipo | Descrição |
|---------|---------|---------|
| data | String | Data da observação |
| valor | String | Valor do IPCA |

---

# Camada Bronze

## bronze_cdi

**Descrição:** Dados brutos do CDI após a ingestão.

| Campo | Tipo |
|---------|---------|
| data | String |
| valor | String |
| data_ingestao | Timestamp |
| arquivo_origem | String |

---

## bronze_selic

**Descrição:** Dados brutos da Selic após a ingestão.

| Campo | Tipo |
|---------|---------|
| data | String |
| valor | String |
| data_ingestao | Timestamp |
| arquivo_origem | String |

---

## bronze_ipca

**Descrição:** Dados brutos do IPCA após a ingestão.

| Campo | Tipo |
|---------|---------|
| data | String |
| valor | String |
| data_ingestao | Timestamp |
| arquivo_origem | String |

---

# Camada Silver

## dim_calendario

**Descrição:** Dimensão temporal utilizada para enriquecimento das análises.

| Campo | Tipo |
|---------|---------|
| data_referencia | Date |
| ano | Integer |
| semestre | Integer |
| trimestre | Integer |
| mes | Integer |
| nome_mes | String |
| ano_mes | String |

---

## silver_indicadores_macro

**Descrição:** Tabela consolidada contendo CDI, Selic e IPCA em granularidade mensal.

| Campo | Tipo |
|---------|---------|
| data_referencia | Date |
| ano_mes | String |
| cdi_percentual | Double |
| selic_percentual | Double |
| ipca_percentual | Double |

---

## silver_quality_report

**Descrição:** Relatório resumido de qualidade dos dados.

| Campo | Tipo |
|---------|---------|
| indicador | String |
| quantidade_registros | Integer |
| data_inicial | Date |
| data_final | Date |

---

# Camada Gold

## gold_evolucao_indicadores

**Descrição:** Estrutura analítica utilizada para análise da evolução histórica dos indicadores econômicos.

| Campo | Tipo |
|---------|---------|
| ano_mes | String |
| cdi_percentual | Double |
| selic_percentual | Double |
| ipca_percentual | Double |

---

## gold_risco_indexadores

**Descrição:** Tabela de métricas estatísticas utilizada para avaliação da volatilidade dos indicadores.

| Campo | Tipo |
|---------|---------|
| indicador | String |
| media | Double |
| minimo | Double |
| maximo | Double |
| desvio_padrao | Double |

---

# Observações

As tabelas **gold_financiamento_indexado** e **gold_simulacao_sac** foram inicialmente consideradas durante a fase de modelagem da solução, porém não foram implementadas devido às limitações metodológicas identificadas na série SGS 4389 utilizada para representar o CDI.

A modelagem adotada segue uma abordagem dimensional simplificada, inspirada no conceito de Star Schema. Neste contexto, a tabela **silver_indicadores_macro** atua como estrutura central para armazenamento dos indicadores econômicos, enquanto a tabela **dim_calendario** fornece os atributos temporais utilizados nas análises realizadas nas camadas Silver e Gold.
