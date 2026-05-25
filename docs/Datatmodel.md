1. Purpose
This document defines the data model for the Water Delivery Network Analytics Platform.
It describes:

Entities

Relationships

Table schemas

Data lineage

Data quality rules

Future extensibility

The model follows a Lakehouse architecture with Bronze → Silver → Gold layers.

2. Conceptual Data Model
This high‑level model shows the core business entities and how they relate.



Core Entities
Telemetry (time‑series operational data)

Weather (external environmental data)

Assets (pumps, channels, gates, meters)

Forecasts (predicted demand)

Risk Scores (anomaly detection outputs)

KPIs (operational metrics for dashboards)

3. Logical Data Model
This section defines the logical structure of each entity, independent of storage technology.

3.1 Telemetry
Represents time‑series readings from channels and pumps.

Field	Type	Description
telemetry_id	string	Unique ID (generated)
timestamp	datetime	Reading timestamp
channel_id	string	Channel identifier
asset_id	string	Pump/gate/meter ID (nullable)
flow_rate	float	L/s
pump_pressure	float	kPa
water_level	float	m
quality_flag	string	OK / Missing / Outlier


3.2 Weather
Represents daily or hourly weather conditions.

Field	Type
weather_id	string
date	date
region	string
rainfall	float
temp_max	float
temp_min	float
evaporation	float


3.3 Assets
Represents physical infrastructure.

Field	Type
asset_id	string
channel_id	string
asset_type	string
install_date	date
condition_score	int
status	string


3.4 Forecasts
Represents predicted water demand.

Field	Type
forecast_id	string
channel_id	string
forecast_date	date
predicted_demand	float
lower_ci	float
upper_ci	float
model_version	string


3.5 Risk Scores
Represents anomaly detection outputs.

Field	Type
risk_id	string
asset_id	string
timestamp	datetime
risk_score	float
anomaly_flag	boolean
model_version	string


3.6 KPIs
Represents aggregated operational metrics.

Field	Type
kpi_id	string
channel_id	string
date	date
avg_flow	float
max_pressure	float
anomaly_count	int


4. Physical Data Model (Lakehouse)




4.1 Bronze Layer (Raw)
Bronze Tables
Table	Description
bronze_telemetry	Raw telemetry from SCADA/CSV
bronze_weather	Raw weather API data
bronze_assets	Raw asset metadata (TechOne export)


Bronze Schema Example
Code
bronze_telemetry/
    timestamp: string
    channel_id: string
    flow_rate: string
    pump_pressure: string
    water_level: string
    source_file: string
    ingest_time: timestamp
4.2 Silver Layer (Cleaned & Enriched)
Silver Tables
Table	Description
silver_telemetry_clean	Cleaned telemetry
silver_weather_enriched	Weather with derived features
silver_asset_joined	Assets joined with telemetry


Silver Schema Example
Code
silver_telemetry_clean/
    timestamp: timestamp
    channel_id: string
    asset_id: string
    flow_rate: float
    pump_pressure: float
    water_level: float
    quality_flag: string
4.3 Gold Layer (Curated Analytics)
Gold Tables
Table	Description
gold_forecasts	Forecast outputs
gold_risk_scores	Anomaly detection outputs
gold_kpis	Aggregated metrics


Gold Schema Example
Code
gold_forecasts/
    channel_id: string
    forecast_date: date
    predicted_demand: float
    lower_ci: float
    upper_ci: float
    model_version: string
5. Data Lineage


Telemetry Lineage
Raw CSV → Bronze → Cleaned Silver → Feature table → Forecast model → Gold outputs

Asset Lineage
TechOne export → Bronze → Silver → Joined with telemetry → Risk model → Gold outputs

Weather Lineage
API → Bronze → Silver → Feature table → Forecast model → Gold outputs

6. Data Quality Rules
6.1 Telemetry
Rule	Description
DQ‑01	timestamp must be valid datetime
DQ‑02	flow_rate must be ≥ 0
DQ‑03	pressure must be within physical limits
DQ‑04	missing values flagged as quality_flag = 'Missing'


6.2 Weather
Rule	Description
DQ‑05	rainfall ≥ 0
DQ‑06	evaporation ≥ 0
DQ‑07	temp_min ≤ temp_max


6.3 Assets
Rule	Description
DQ‑08	asset_id must be unique
DQ‑09	condition_score between 1–5


7. Keys & Constraints
Primary Keys
telemetry_id

weather_id

asset_id

forecast_id

risk_id

kpi_id

Foreign Keys
telemetry.channel_id → assets.channel_id

risk_scores.asset_id → assets.asset_id

forecasts.channel_id → telemetry.channel_id

8. Future Extensibility
8.1 Integration with TechnologyOne
Future tables:

work_orders

maintenance_history

asset_costs

8.2 Real‑time streaming
Add:

streaming_telemetry

streaming_anomalies

8.3 Optimisation models
Add:

pump_schedule_optimisation

water_allocation_plans

9. Summary
This data model:

Supports forecasting, anomaly detection, and dashboards

Follows Lakehouse best practices

Ensures governance, quality, and scalability

Is extensible for future MI integrations