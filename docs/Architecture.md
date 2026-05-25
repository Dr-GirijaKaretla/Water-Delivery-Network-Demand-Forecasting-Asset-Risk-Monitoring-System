Architecture.md — Water Delivery Network Demand Forecasting & Asset Risk Monitoring System
1. Purpose
This document describes the technical architecture of the Water Delivery Network Demand Forecasting & Asset Risk Monitoring System.

It covers:

Overall solution design

Lakehouse architecture (Bronze → Silver → Gold)

Data flows and components

MLOps and monitoring

Security, governance, and extensibility

This is written to the level expected for an enterprise data product at MI.

2. High‑level architecture overview
2.1 Conceptual view
Goal:  
Ingest operational data (telemetry, weather, asset metadata), transform it into governed analytical datasets, and deliver:

Forecasts of water demand

Risk scores and anomalies for assets

Dashboards and alerts for stakeholders

Core layers:

Source Systems

Telemetry/SCADA (flows, pressures, levels)

Weather API (e.g., BOM)

Asset metadata (ERP/EAM, e.g., TechnologyOne)

Data Platform (Lakehouse)

Azure Data Lake Gen2

Azure Databricks (PySpark, MLflow)

Bronze / Silver / Gold tables

Analytics & Models

Forecasting models (Prophet / ARIMA / LSTM)

Anomaly detection (Isolation Forest / statistical thresholds)

Consumption

Power BI dashboards

Alerts (email/Teams/webhooks)

API/SQL endpoints (optional future)

3. Logical architecture
3.1 Lakehouse layers
Bronze (Raw)

Raw telemetry, weather, and asset metadata

Minimal transformation (schema alignment, ingestion metadata)

Append‑only, immutable

Silver (Refined)

Cleaned, validated, enriched datasets

Joins between telemetry, weather, and asset metadata

Feature‑ready tables for modelling

Gold (Curated)

Forecast outputs per channel

Asset risk scores and anomaly flags

Operational KPIs for dashboards

3.2 Core data entities
Telemetry: time‑series readings per channel/asset

Weather: daily/hourly weather per region

Assets: pumps, channels, gates, meters, etc.

Forecasts: predicted demand per channel/date

Risks: risk scores and anomaly flags per asset/timestamp

4. Physical architecture
4.1 Cloud components (Azure)
Azure Data Lake Storage Gen2

Stores Bronze/Silver/Gold data

Hierarchical namespace for Lakehouse layout

Azure Databricks

Notebooks for EDA, feature engineering, modelling

Jobs for scheduled pipelines

MLflow for experiment tracking and model registry

Azure Data Factory (or Databricks Workflows)

Orchestrates ingestion and transformation pipelines

Schedules daily/hourly runs

Power BI

Connects to Gold layer (via DirectQuery or import)

Delivers dashboards to planners, operators, asset managers

GitHub

Source control for notebooks, pipelines, infra as code

GitHub Actions for CI/CD

5. Data flow
5.1 End‑to‑end flow
Ingestion (Bronze)

Telemetry CSV/Parquet files land in data/raw/telemetry/

Weather data ingested via API → data/raw/weather/

Asset metadata exported from ERP/EAM → data/raw/assets/

bronze_ingest.py validates schema and writes to Bronze tables

Transformation (Silver)

silver_transform.py cleans and validates data

Handles missing values, outliers, unit conversions

Joins telemetry with weather and asset metadata

Writes feature‑ready Silver tables

Modelling (Gold)

Forecasting notebook reads Silver tables

Trains/updates models (Prophet/ARIMA/LSTM)

Writes forecast outputs to Gold tables

Anomaly detection notebook computes risk scores and flags

Writes risk tables to Gold

Consumption

Power BI connects to Gold tables

Dashboards show forecasts, risks, KPIs

Alerts can be triggered based on thresholds (e.g., high risk_score)

6. Components
6.1 Pipelines
bronze_ingest.py

Input: raw telemetry, weather, asset files

Output: Bronze tables

Responsibilities: schema enforcement, ingestion metadata

silver_transform.py

Input: Bronze tables

Output: Silver tables

Responsibilities: cleaning, validation, enrichment, feature creation

gold_publish.py

Input: Silver tables + model outputs

Output: Gold tables (forecasts, risks, KPIs)

Responsibilities: publishing curated datasets for BI and APIs

6.2 Notebooks
01_eda.ipynb — exploratory analysis of telemetry and weather

02_feature_engineering.ipynb — feature creation for models

03_forecasting_model.ipynb — training and evaluating forecasting models

anomaly_detection.ipynb (future) — training and evaluating risk models

6.3 MLOps
MLflow

Tracks experiments (params, metrics, artifacts)

Registers best models in Model Registry

Stores model versions for rollback

Monitoring

Model performance metrics (MAPE, precision/recall for anomalies)

Data drift checks (distribution shifts in key features)

Retraining triggers based on performance thresholds

7. Security and governance
7.1 Identity & access
Azure AD for authentication

RBAC for role‑based access to Databricks, ADLS, ADF

Separate dev / test / prod workspaces and storage accounts

7.2 Data governance
Unity Catalog (if enabled) for:

Centralised data access control

Data lineage

Table and column‑level permissions

Data classification

Operational telemetry: internal

Asset metadata: internal, potentially sensitive

Aggregated Gold tables: safe for broader consumption

7.3 Compliance
Encryption at rest (ADLS, Databricks)

Encryption in transit (HTTPS, TLS)

8. Monitoring and observability
8.1 Pipeline monitoring
ADF/Databricks job run status

Logging of row counts, error rates, and latency

Alerts on pipeline failures

8.2 Model monitoring
Forecast accuracy over time (e.g., MAPE per channel)

Anomaly detection performance (false positives/negatives)

Drift metrics on key features (e.g., flow_rate distribution)

8.3 Dashboard usage
Power BI usage metrics (views, active users)

Helps prioritise enhancements and stakeholder engagement

9. Extensibility and future integrations
9.1 Integration with TechnologyOne (ERP/EAM)
Future phases can integrate directly with MI’s ERP/EAM (e.g., TechnologyOne) to:

Pull asset registers and work orders automatically

Push risk scores back into EAM for maintenance planning

Link financials and budgeting with asset risk and demand forecasts

9.2 Real‑time / near real‑time
Introduce streaming ingestion (Event Hubs / Kafka) for near real‑time telemetry

Use Structured Streaming in Databricks for continuous anomaly detection

9.3 Advanced analytics
Failure prediction models (time‑to‑failure)

Optimisation models for water allocation and pump scheduling

10. Summary
This architecture:

Follows a Lakehouse pattern suitable for MI’s environment

Separates concerns across Bronze/Silver/Gold layers

Supports scalable forecasting and risk detection

Embeds MLOps, monitoring, and governance from the start

Is designed to integrate with MI’s existing systems (e.g., TechnologyOne) in later phases

If you want, next I can:

Draft Sprint 0 / Foundation as a formal delivery plan

Generate the bronze_ingest.py pipeline code

Create DataModel.md with full table schemas and relationships