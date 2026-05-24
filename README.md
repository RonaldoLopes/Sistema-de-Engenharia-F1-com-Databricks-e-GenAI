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

Este projeto implementa uma **plataforma completa de engenharia de dados** voltada para análise da Fórmula 1, utilizando o ecossistema **Databricks Lakehouse** aliado a **Inteligência Artificial Generativa (GenAI)**.

A arquitetura segue o padrão **Medallion Architecture (Bronze → Silver → Gold)**, com pipelines orquestrados via **Delta Live Tables (DLT)** e governança centralizada no **Unity Catalog**.

------------------------------------------------------------------------

## 🎯 Objetivos

- Construir um Data Lakehouse escalável e governado
- Integrar dados históricos da Ergast API (Jolpica)
- Aplicar ETL/ELT com Apache Spark e Delta Live Tables
- Disponibilizar data marts prontos para BI e ML
- Gerar relatórios narrativos com GenAI
- Garantir qualidade de dados com DLT Expectations

------------------------------------------------------------------------

## 🏗️ Arquitetura do Sistema

```
┌─────────────────────────────────────────────────────────────────────┐
│                         FONTES DE DADOS                             │
├───────────────┬─────────────────┬────────────────┬─────────────────┤
│   Ergast API  │   OpenF1.org    │     FastF1      │     Kaggle      │
│   (Jolpica)   │      API        │   Biblioteca    │    Datasets     │
└───────┬───────┴────────┬────────┴────────┬───────┴────────┬────────┘
        │                │                 │                 │
        └────────────────┴─────────────────┴─────────────────┘
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  DELTA LIVE TABLES (DLT) PIPELINE                   │
│                    Catálogo: f1_lakehouse                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  🥉 BRONZE — pipelines/01_bronze/ergast_dlt.ipynb                  │
│  • drivers_raw       Pilotos brutos da API                         │
│  • constructors_raw  Construtores brutos da API                    │
│  • races_raw         Corridas brutas da API                        │
│  • results_raw       Resultados brutos da API                      │
│                                                                     │
│  🥈 SILVER — pipelines/02_silver/transformations.ipynb             │
│  • drivers           Pilotos limpos e deduplicados                 │
│  • constructors      Construtores limpos                           │
│  • races             Corridas padronizadas                         │
│  • results           Resultados enriquecidos com joins             │
│                                                                     │
│  🥇 GOLD — pipelines/03_gold/                                      │
│  • driver_standings    Classificação do campeonato por temporada   │
│  • season_progression  Evolução corrida a corrida de cada piloto   │
│  • race_summary_facts  Fatos agregados por corrida (GP)            │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
   📊 Power BI           📈 Tableau             🤖 GenAI
   Dashboards            Dashboards        Relatórios automáticos
```

------------------------------------------------------------------------

## 📂 Estrutura do Projeto

```
├── pipelines/
│   ├── 01_bronze/
│   │   └── ergast_dlt.ipynb          # Ingestão DLT da Ergast API
│   ├── 02_silver/
│   │   └── transformations.ipynb     # Limpeza e enriquecimento DLT
│   └── 03_gold/
│       ├── driver_standings.ipynb    # Classificação por temporada
│       ├── season_progression.ipynb  # Evolução corrida a corrida
│       └── race_summary_facts.ipynb  # Fatos resumidos por GP
├── setup/
│   └── 00_initial_setup.ipynb        # Configuração inicial do ambiente
├── databricks.yml                    # Bundle DAB (dev/prod)
└── docs/
    └── documentation.html            # Documentação técnica completa
```

------------------------------------------------------------------------

## 🥇 Camada Gold — Tabelas

### `gold.driver_standings`
Classificação dos pilotos ao final de cada temporada.

| Coluna | Tipo | Descrição |
|---|---|---|
| `season` | int | Temporada |
| `championship_position` | int | Posição no campeonato |
| `driver_id` / `driver_name` | string | Identificação do piloto |
| `nationality` | string | Nacionalidade |
| `constructor_id` | string | Equipe principal na temporada |
| `total_points` | float | Pontos totais |
| `points_gap_to_leader` | float | Diferença para o líder |
| `total_races` | int | Corridas disputadas |
| `wins` / `podiums` | int | Vitórias e pódios |
| `dnfs` | int | Não-finalizações |
| `best_finish` / `avg_finish_position` | int/float | Melhor e média de posição |
| `fastest_laps` | int | Voltas mais rápidas conquistadas |

---

### `gold.season_progression`
Evolução corrida a corrida de cada piloto dentro da temporada.

| Coluna | Tipo | Descrição |
|---|---|---|
| `season` / `round` | int | Temporada e rodada |
| `race_name` / `race_date` | string/date | Identificação do GP |
| `driver_id` / `driver_name` | string | Piloto |
| `constructor_id` | string | Equipe |
| `points_in_race` | float | Pontos nessa corrida |
| `cumulative_points` | float | Pontos acumulados até essa rodada |
| `championship_position` | int | Posição no campeonato após a rodada |
| `wins_so_far` / `podiums_so_far` / `dnfs_so_far` | int | Acumulados até a rodada |
| `gap_to_leader` | float | Diferença para o líder naquele momento |

---

### `gold.race_summary_facts`
Um registro por corrida com os principais fatos do GP.

| Coluna | Tipo | Descrição |
|---|---|---|
| `season` / `round` | int | Temporada e rodada |
| `race_name` / `race_date` / `circuit_id` | string/date | Identificação |
| `winner_driver_id/name/constructor_id` | string | Vencedor |
| `winning_time` / `winning_margin_ms` | string/long | Tempo e margem de vitória |
| `pole_driver_id` / `pole_driver_name` | string | Pole position |
| `fastest_lap_driver_id/name/time/speed` | string/float | Volta mais rápida |
| `race_laps` | int | Total de voltas |
| `total_starters` / `total_classified` / `dnf_count` | int | Estatísticas de participação |
| `total_points_awarded` | float | Pontos distribuídos na corrida |
| `avg_grid_position` | float | Posição média de largada |

------------------------------------------------------------------------

## 📊 Fontes de Dados

| Fonte | Tipo | Uso |
|---|---|---|
| Ergast API (Jolpica) | REST / JSON | Dados históricos (1950–2024) |
| OpenF1.org | REST / JSON | Dados em tempo real |
| FastF1 | Biblioteca Python | Telemetria e timing |
| Kaggle | CSV | Datasets complementares |

------------------------------------------------------------------------

## 🤖 GenAI

- Relatórios automáticos de corridas e temporadas
- Insights narrativos sobre desempenho de pilotos
- Análise comparativa entre temporadas

------------------------------------------------------------------------

## ⚙️ Tecnologias

| Tecnologia | Papel |
|---|---|
| Databricks | Plataforma de execução e orquestração |
| Delta Live Tables | Pipelines declarativos com qualidade de dados |
| Apache Spark | Processamento distribuído |
| Delta Lake | Formato de armazenamento ACID |
| Unity Catalog | Governança e catálogo de dados |
| Python 3.10+ | Linguagem principal |
| OpenAI | Geração de relatórios com GenAI |
| Power BI / Tableau | Visualização e dashboards |

------------------------------------------------------------------------

## 🚀 Evoluções Futuras

- Streaming em tempo real com OpenF1
- Machine Learning para previsão de resultados
- Feature Store para modelos de ML
- Integração com FastF1 para telemetria

------------------------------------------------------------------------

## 📌 Conclusão

Plataforma moderna que transforma dados históricos e em tempo real da F1 em insights acionáveis, combinando engenharia de dados robusta com inteligência artificial generativa.
