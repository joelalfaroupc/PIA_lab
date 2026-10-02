# SAMO — Agrovoltaic Decision Support

**A sensor-based prototype for exploring the balance between solar production and crop conditions.**

SAMO combines historical agrovoltaic sensor data, interpretable tracker-rotation rules, offline learning and a Streamlit dashboard. The project turns successive university sprints into an inspectable decision-support workflow.

![Agrovoltaic dashboard](capturas%20dashboard/dashboard_estado_general.png)

## What the project delivers

- Sensor exploration and normalization across light, soil moisture, soil temperature, climate and tracker angles.
- Integrated datasets at six-hour and ten-minute resolutions.
- Candidate rotation rules and interpretable modelling with decision trees and ElasticNet.
- A dashboard with operational views, recommendations, crop settings, simulation and alerts.
- An offline DQN policy and an LSTM world model for exploring alternative decisions.

## Implementation

| Stage | Main work | Where to look |
| --- | --- | --- |
| Sprint 1 | Exploratory analysis and sensor diagnostics. | [sprint1](sprint1/) |
| Sprint 2 | Six-hour integration, modelling, energy/crop indicators and six candidate rotation rules. | [modelling notebook](sprint2/sprint2_modelizacion_agrovoltaica.ipynb) |
| Sprint 3 | Ten-minute pipeline, reusable decision modules, dashboard and offline simulation. | [application](sprint3/src/app.py), [core modules](sprint3/src/core/) |

The rule engine exposes understandable recommendations, while the learning components support comparison with historical operation. An IEC indicator summarizes the energy/crop tradeoff used in the analysis.

The Streamlit interface organizes state, time series, recommendations, crop conditions, LSTM simulation, decision-support scenarios and alerts. Constant tracker readings are flagged for inspection; a flat signal alone does not establish a physical sensor fault.

![Simulation view](capturas%20dashboard/dashboard_simulacion.png)

## Results and interpretation

The [offline evaluation report](sprint3/reports/validacion_offline_mejora_energia_cultivo.md) records 23,043 comparable ten-minute observations over 241 days. Its historical comparison estimates mean IEC increasing from **0.258 to 0.320 (+24.2%)** under the evaluated DQN recommendations.

This is an **offline counterfactual estimate**, based on comparable historical actions. It is not a measured increase in field energy production or crop yield. Raw saturated Q-values are not a reliable quality score; the report's comparison and assumptions are the relevant evidence.

The concrete outputs include an integrated dataset, candidate policies, modelling artifacts, evaluation reports and an interactive application. Live equipment control and field validation are outside the demonstrated scope.

## Run the dashboard

Python 3.11 is a practical starting point for the pinned NumPy dependency. From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r sprint3/src/requirements.txt
python -m pip install scikit-learn joblib torch
cd sprint3
streamlit run src/app.py
```

The extra packages support the modelling and LSTM modules; their versions are not pinned in the application requirements. On Windows, activate with `.venv\Scripts\activate`.

Keep the repository layout intact: data loading can reuse Sprint 2 outputs when Sprint 3 artifacts are unavailable. The dashboard therefore depends on the data/model files as well as the application code.

## Tests and further reading

From the repository root:

```bash
python -m pytest sprint3/src/tests
```

The test suite covers rules, pipeline utilities and application/model components. This documentation update does not report a fresh run of the application or test suite.

- [Sprint 3 guide](sprint3/README.md)
- [Source modules and tests](sprint3/src/)
- [Versioned outputs](sprint3/outputs/)
- [Reports](sprint3/reports/)
- [Dashboard captures](capturas%20dashboard/)

## Project context

University team project, preserved from [danielalvarezsarroca/PIA_lab](https://github.com/danielalvarezsarroca/PIA_lab), with its original history, team documentation and credits. This portfolio edition presents the project as a whole and distinguishes implemented functionality from offline estimates.
