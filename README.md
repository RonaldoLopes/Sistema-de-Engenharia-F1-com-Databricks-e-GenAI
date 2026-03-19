# 🏎️ Sistema de Engenharia de Dados da Fórmula 1 com Databricks e GenAI

<p align="center">
  <img src="https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=Databricks&logoColor=white"/>
  <img src="https://img.shields.io/badge/Apache_Spark-FFFFFF?style=for-the-badge&logo=apachespark&logoColor=#E35A16"/>
  <img src="https://img.shields.io/badge/Delta_Lake-000000?style=for-the-badge&logo=delta&logoColor=white"/>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white"/>
</p>

------------------------------------------------------------------------

## 📋 Sobre o Projeto

Este projeto implementa uma **plataforma completa de engenharia de
dados** voltada para análise da Fórmula 1, utilizando o ecossistema
**Databricks Lakehouse** aliado a **Inteligência Artificial Generativa
(GenAI)**.

A arquitetura segue o padrão **Medallion Architecture (Bronze, Silver,
Gold)**, com orquestração via **Databricks Workflows**.

------------------------------------------------------------------------

## 🎯 Objetivos

-   Construir um Data Lakehouse escalável\
-   Integrar múltiplas fontes de dados\
-   Aplicar ETL/ELT com Apache Spark\
-   Disponibilizar dados para BI e ML\
-   Gerar relatórios com GenAI\
-   Garantir governança com Unity Catalog

------------------------------------------------------------------------

## 🏗️ Arquitetura do Sistema

    ┌─────────────────────────────────────────────────────────────────────┐
    │                         FONTES DE DADOS                             │
    ├───────────────┬─────────────────┬────────────────┬─────────────────┤
    │   Ergast API  │   OpenF1.org    │     FastF1      │     Kaggle      │
    │   (Jolpica)   │      API        │   Biblioteca    │    Datasets     │
    └───────┬───────┴────────┬────────┴────────┬───────┴────────┬────────┘
            │                 │                  │                │
            └─────────────────┼──────────────────┼────────────────┘
                              ▼
    ┌─────────────────────────────────────────────────────────────────────┐
    │                    DATABRICKS WORKFLOWS                             │
    ├─────────────────────────────────────────────────────────────────────┤
    │                                                                     │
    │  🥉 BRONZE                                                         │
    │  • Ingestão de dados brutos (JSON/CSV)                             │
    │  • Armazenamento em Delta Lake                                     │
    │                                                                     │
    │  🥈 SILVER                                                         │
    │  • Limpeza e padronização                                          │
    │  • Deduplicação e enriquecimento                                   │
    │                                                                     │
    │  🥇 GOLD                                                           │
    │  • Agregações de negócio                                           │
    │  • Data marts para consumo                                         │
    │                                                                     │
    └─────────────────────────────────────────────────────────────────────┘
                                  │
            ┌─────────────────────┼─────────────────────┐
            ▼                     ▼                     ▼
       📊 Power BI           📈 Tableau             🤖 GenAI
       Dashboards            Dashboards        Relatórios automáticos

------------------------------------------------------------------------

## 📊 Fontes de Dados

-   Ergast API (histórico)\
-   OpenF1 (tempo real)\
-   FastF1 (telemetria)\
-   Kaggle (datasets)

------------------------------------------------------------------------

## 🤖 GenAI

-   Relatórios automáticos\
-   Insights narrativos\
-   Explicações de desempenho

------------------------------------------------------------------------

## ⚙️ Tecnologias

-   Databricks\
-   Apache Spark\
-   Delta Lake\
-   Python\
-   OpenAI\
-   Power BI / Tableau

------------------------------------------------------------------------

## 🚀 Evoluções Futuras

-   Streaming em tempo real\
-   Machine Learning\
-   Feature Store

------------------------------------------------------------------------

## 📌 Conclusão

Plataforma moderna que transforma dados da F1 em insights acionáveis.
