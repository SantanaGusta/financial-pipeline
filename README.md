
# 🚀 Pipeline de Engenharia de Dados End-to-End: Lakehouse Financeiro com Databricks

> Um projeto de arquitetura de dados moderna simulando um gateway de pagamentos de alta volumetria, construído com **Databricks, PySpark, Delta Lake e a Arquitetura Medallion**.

---

## 🏗️ Diagrama de Arquitetura (Medallion Architecture)

O pipeline foi desenhado para processar transações financeiras de forma estruturada, garantindo rastreabilidade, qualidade de dados e alta performance analítica.

```text
[ API de Pagamentos / Simulação de Carga ] 
               │
               ▼ (Extração e Persistência Bruta)
    ┌──────────────────────┐
    │     CAMADA BRONZE    │ ──> Armazenamento imutável (JSON / Delta Lake)
    └──────────────────────┘
               │
               ▼ (Limpeza, Tipagem, Tratamento de Nulos & Deduplicação)
    ┌──────────────────────┐
    │     CAMADA SILVER    │ ──> Dados limpos, padronizados e otimizados (Delta Lake)
    └──────────────────────┘
               │
               ▼ (Modelagem Dimensional, Star Schema & Agregações de Negócio)
    ┌──────────────────────┐
    │      CAMADA GOLD     │ ──> Tabelas Fato e Dimensão prontas para BI / Analytics
    └──────────────────────┘

```

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **Processamento Distribuído:** Apache Spark & PySpark
* **Storage & Lakehouse:** Delta Lake / Databricks FS (DBFS)
* **Linguagem:** Python 3
* **Conceitos Aplicados:** Data Lakehouse, Arquitetura Medallion, Modelagem Dimensional (Star Schema), Limpeza de Dados e Otimização de Storage.

---

## 📂 Estrutura do Repositório

```text
financial-pipeline-databricks/
│
├── README.md                      # Documentação detalhada do projeto
├── notebooks/
│   ├── 01_bronze_ingestion.py     # Ingestão de dados crus (Simulação de API)
│   ├── 02_silver_transform.py     # Limpeza, tipagem e gravação na Silver
│   └── 03_gold_aggregation.py     # Modelagem Star Schema e métricas (Gold)

```

---

## 🔄 Detalhes Técnicos das Camadas

### 1. Camada Bronze (`01_bronze_ingestion.py`)

* **Objetivo:** Capturar os dados no estado mais bruto possível (*as-is*), garantindo auditoria e reprodutibilidade.
* **Funcionalidade:** Simula a extração em lote de transações financeiras de um gateway de pagamentos, contendo metadados como IDs de transação, usuários, valores em moeda, status de aprovação, categorias de estabelecimentos (*merchants*) e dados de dispositivos.
* **Persistência:** Os registros são convertidos em um DataFrame Spark e salvos diretamente no formato **Delta Lake** na raiz da camada bronze.

### 2. Camada Silver (`02_silver_transform.py`)

* **Objetivo:** Refinar, limpar e padronizar os dados brutos, preparando-os para consumo analítico confiável.
* **Funcionalidade:**
* Leitura otimizada das tabelas Delta da camada Bronze.
* Conversão e tipagem correta de colunas (datas, valores numéricos e chaves).
* Tratamento de valores nulos e remoção de duplicatas (`dropDuplicates`) baseada no ID da transação.
* Aplicação de regras de normalização de texto e filtros de consistência operacional.


* **Persistência:** Salvo em formato otimizado Delta Lake na camada Silver.

### 3. Camada Gold (`03_gold_aggregation.py`)

* **Objetivo:** Entregar valor de negócio através de modelagem dimensional e visões consolidadas.
* **Funcionalidade:**
* Criação de tabelas agregadas orientadas a métricas de negócio.
* Cálculo de indicadores-chave de desempenho (KPIs) como:
* Faturamento total e ticket médio por categoria de estabelecimento e método de pagamento.
* Taxa de conversão de pagamentos aprovados versus recusados ou suspeitos de fraude.
* Volumetria transacional agrupada por períodos temporais.





---

## 🚀 Como Executar o Projeto

1. Acesse o seu workspace do **Databricks** (ou Databricks Community Edition).
2. Crie um cluster Spark utilizando uma versão recente do Databricks Runtime.
3. Importe os notebooks disponíveis na pasta `notebooks/` para o seu workspace.
4. Execute os notebooks sequencialmente:
* **Passo 1:** `01_bronze_ingestion` (Responsável por popular a camada crua)
* **Passo 2:** `02_silver_transform` (Responsável pela limpeza e tipagem)
* **Passo 3:** `03_gold_aggregation` (Responsável pelas visões de negócio)



---

*Projeto desenvolvido para fins de portfólio e demonstração de competências avançadas em Engenharia de Dados moderna.*

```
