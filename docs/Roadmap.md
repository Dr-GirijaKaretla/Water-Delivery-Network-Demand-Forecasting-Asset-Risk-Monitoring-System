. Purpose
This roadmap outlines the phased delivery plan for the Water Delivery Network Analytics Platform.
It provides:

Delivery phases

Milestones

Sprint breakdown

Dependencies

Risks

Enhancement pathways

The roadmap is designed for enterprise delivery and aligns with MI’s operational environment.

2. High‑Level Delivery Phases




Phase 0 — Foundation (Week 1)
Set up repo, environments, sample data, and initial pipelines.

Phase 1 — Data Layer (Weeks 2–3)
Build Bronze/Silver/Gold pipelines and validate data quality.

Phase 2 — Modelling (Weeks 4–6)
Develop forecasting and anomaly detection models with MLflow.

Phase 3 — Dashboards & MLOps (Weeks 7–9)
Build Power BI dashboards, monitoring, and CI/CD.

Phase 4 — Pilot & Feedback (Weeks 10–12)
Deploy MVP, gather feedback, refine models and dashboards.

Phase 5 — Production Hardening (Weeks 13–16)
Security, governance, performance tuning, documentation.

Phase 6 — Enhancements (Ongoing)
TechOne integration, optimisation models, real‑time telemetry.

3. Detailed Roadmap by Phase
Phase 0 — Foundation (Week 1)
Goal: Establish the project skeleton and development environment.

Deliverables
GitHub repo created

Repo structure added

BRD, Architecture, DataModel, Roadmap

Sample datasets (telemetry, weather, assets)

Bronze ingestion pipeline (initial version)

EDA notebook

Dependencies
GitHub access

Databricks workspace

ADLS Gen2 storage

Next Step
Generate Bronze ingestion pipeline

Phase 1 — Data Layer (Weeks 2–3)
Goal: Build the Lakehouse foundation.

Deliverables
Bronze → Silver → Gold pipelines

Data validation rules

Feature engineering tables

Weather enrichment

Asset join logic

Data quality monitoring

Milestones
Silver telemetry ready

Silver weather ready

Silver asset join ready

Gold KPIs ready

Risks
Poor data quality

Missing timestamps

Inconsistent asset metadata

Phase 2 — Modelling (Weeks 4–6)
Goal: Build forecasting and anomaly detection models.

Deliverables
Forecasting model (Prophet/ARIMA/LSTM)

Anomaly detection model (Isolation Forest/statistical)

Model evaluation metrics

MLflow experiment tracking

Model registry entries

Milestones
Forecast accuracy < 10% MAPE

Anomaly detection false positives < 15%

Model versioning enabled

Dependencies
Clean Silver data

Feature engineering complete

Next Step
Generate forecasting model notebook

Phase 3 — Dashboards & MLOps (Weeks 7–9)
Goal: Deliver operational insights and monitoring.

Deliverables
Power BI dashboards

Forecast vs Actual

Asset Health

Anomaly Alerts

Channel KPIs

Drift monitoring

Retraining triggers

CI/CD pipelines (GitHub Actions)

Milestones
Dashboards published

Monitoring active

Automated retraining enabled

Risks
Dashboard refresh delays

Model drift detection thresholds too sensitive

Next Step
Generate Power BI dashboard spec

Phase 4 — Pilot & Feedback (Weeks 10–12)
Goal: Deploy MVP to MI stakeholders.

Deliverables
MVP deployed

User training sessions

Feedback collection

Model tuning

Dashboard refinement

Milestones
Operator adoption > 80%

Forecast accuracy validated in real operations

Risks
Low stakeholder engagement

Misalignment with operational workflows

Phase 5 — Production Hardening (Weeks 13–16)
Goal: Make the system production‑ready.

Deliverables
Security hardening

RBAC roles

Unity Catalog governance

Performance tuning

Documentation

Handover package

Milestones
99.5% pipeline uptime

Full auditability

Production sign‑off

Phase 6 — Enhancements (Ongoing)
Goal: Extend the platform with advanced capabilities.

Enhancement Opportunities
TechOne Integration

Work orders

Asset lifecycle

Maintenance history

Real‑time Telemetry

Event Hubs / Kafka

Streaming anomaly detection

Optimisation Models

Pump scheduling

Water allocation optimisation

Advanced Analytics

Failure prediction

Cost‑risk modelling

Next Step
Map TechOne modules to data product

4. Sprint Breakdown
Sprint 0 — Foundation (1 week)
Repo setup

Sample data

Bronze ingestion

EDA

Sprint 1 — Data Layer (2 weeks)
Silver transformations

Gold KPIs

Data quality rules

Sprint 2 — Modelling (3 weeks)
Forecasting model

Anomaly detection

MLflow integration

Sprint 3 — Dashboards & MLOps (3 weeks)
Power BI dashboards

Drift monitoring

CI/CD

Sprint 4 — Pilot (2 weeks)
Deploy MVP

Collect feedback

Tune models

Sprint 5 — Production Hardening (3 weeks)
Governance

Security

Documentation

5. Dependencies
Azure subscription

Databricks workspace

ADLS Gen2

Power BI workspace

Access to telemetry, weather, and asset data

GitHub repo + Actions

6. Risks & Mitigations
Risk	Mitigation
Poor data quality	Silver validation + alerts
Model drift	Monitoring + retraining
Access delays	Pre‑approved RBAC roles
Low adoption	Training + stakeholder workshops
Pipeline failures	CI/CD + automated testing


7. Summary
This roadmap provides a structured, phased approach to delivering a production‑ready analytics platform for MI.
It ensures:

Strong foundations

Scalable architecture

Reliable models

Operational dashboards

Governance and monitoring

Clear enhancement pathways