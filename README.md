# Predictive Maintenance & Intelligent Health Indexing of HV Circuit Breakers

An industrial AI/ML system developed to transition electrical substation asset management from traditional time-based schedules to data-driven **Condition-Based Monitoring (CBM)**. This repository implements an end-to-end machine learning pipeline utilizing cost-sensitive gradient boosting to predict insulation, mechanical, and structural degradation in 145kV SF6 circuit breakers using multi-sensor telemetry logs.

## 📊 Industrial Problem & Impact
Substation circuit breakers must operate reliably under extreme fault conditions to protect grid assets. Traditional maintenance strategies rely on fixed calendar intervals or operational counts, leading to significant structural inefficiencies:
* **Over-maintenance:** High operational expenditure (OpEx) spent inspecting healthy assets.
* **Under-maintenance:** Unplanned outages caused by latent mechanical or dielectric failures.

This project deploys a predictive framework using **XGBoost** to analyze multi-variable telemetry waveforms and environmental metrics. It calculates a real-time **Asset Health Index** and estimates the probability of imminent failure, allowing utilities to schedule interventions precisely when required.

## 🛠️ System Architecture & Pipeline
The core backend pipeline processes raw time-series and tabular telemetry data through four distinct stages:

1. **Ingestion & Parsing:** Imports high-frequency sensor streams tracking mechanical movement and environmental status.
2. **Feature Engineering:** Extracts non-linear degradation signals (e.g., cumulative thermal wear calculations and timing profile deviations).
3. **Imbalance Mitigation:** Configures cost-sensitive log-loss optimization to address severe class imbalances (fault occurrences represent <5% of historical operating logs).
4. **Inference & Evaluation:** Outputs fault classifications along with probability scores used to determine asset health rankings.

## 📈 Tracked Technical Features
The model operates on five critical physical and environmental parameters:
* `SF6_Pressure_Bar`: Monitors the integrity of the primary arc-quenching and insulating dielectric medium (nominal ~6.0 bar).
* `Coil_Trip_Time_ms`: Measures mechanical operating latency (nominal ~20–25 ms); increases indicate friction, auxiliary switch issues, or mechanism wear.
* `Contact_Speed_ms`: Tracks contact separation velocity (nominal ~4.5 m/s); slower velocities extend arc duration, leading to severe contact erosion.
* `Cumulative_I2t`: Represents cumulative thermal and electrical stress calculated by integrating squared fault current over arcing duration ($\int I^2 dt$).
* `Operations_Count`: Tracks total mechanical duty cycles to monitor long-term spring and mechanism fatigue.

## 🚀 Repository Structure
```text
pgcil-breaker-predictive-maintenance/
│
├── data/                   # Directory for operational logs (Git-ignored)
├── src/                    # Modular source code
│   ├── __init__.py
│   ├── preprocessing.py    # Feature scaling, imputation, and data splitting
│   ├── model.py            # XGBoost configuration and hyperparameter tuning
│   └── evaluation.py       # Metrics extraction and visualization generators
│
├── .gitignore              # Restricts sensitive corporate data from being pushed
├── main.py                 # Pipeline orchestration script
├── requirements.txt        # Production dependency manifest
└── README.md               # System documentation
