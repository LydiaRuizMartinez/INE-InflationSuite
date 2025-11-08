# Spain Inflation Prediction System

End-to-end platform that **downloads INE data**, **cleans and transforms** it, **trains models** (ARIMA; optional Random Forest and LSTM), **generates 12-month forecasts**, and **reports with charts and a technical PDF**.

## 📌 What it is and why

A solution to **anticipate inflation in Spain** with a reproducible, auditable flow based on official data from the **Instituto Nacional de Estadística (INE)**. Designed for **economics teams**: run → obtain **interval forecasts** → receive **charts** and a **PDF report**.

## 🧩 Architecture

- `src/main.py` → **Orchestrates the full pipeline** (download, cleaning, features, models, forecasting, reporting).
- `src/ine_extractor.py` → INE downloader.
- `src/data_cleaner.py` → Cleaning and normalization of dates/values.
- `src/feature_engineering.py` → Lag features, moving averages, seasonality.
- `src/model_trainer.py` → Trains ARIMA (baseline) and optionally RF/LSTM.
- `src/predictor.py` → Forecasts and intervals.
- `src/report_generator.py` → Charts and **technical PDF**.

**Folder structure**

```
src/                code
data/raw/           INE CSVs downloaded (step 1)
data/processed/     processed CSVs + engineered_features.csv (steps 2–3)
models/             saved models (e.g., arima_model.pkl)
reports/            forecasts (csv/json), visualizations (png), technical PDF
logs/               execution logs
```

## ✅ Requirements

- **Python 3.10–3.12**
- Recommended memory: **4–8 GB**
- Internet connection for INE downloads  
  Dependencies in `requirements.txt` (includes `statsmodels`, `scikit-learn`, and **optional TensorFlow**).

## ⚙️ Installation

```bash
# 1) Virtual environment
python -m venv venv
# Windows
venv\Scripts\Activate.ps1
# macOS/Linux
source venv/bin/activate

# 2) Dependencies
pip install -r requirements.txt
```

## 🗂️ Minimal configuration

Use **`config/config.yaml`** for dates, paths, and INE series. We include **`config/config.example.yaml`** as a template: copy it and adjust.

```yaml
data:
  start_date: "2015-01-01"
  end_date: "2024-12-31"
  urls:
    ipc_general: "https://servicios.ine.es/wstempus/js/es/VALORES_SERIE/{ID_SERIE_IPC_GENERAL}"
    ipc_grupo: "https://servicios.ine.es/wstempus/js/es/VALORES_SERIE/{ID_SERIE_GRUPO}"
    ipca: "https://servicios.ine.es/wstempus/js/es/VALORES_SERIE/{ID_SERIE_IPCA}"

prediction:
  horizon_months: 12 # forecast horizon (months)

paths:
  data:
    raw: "data/raw/"
    processed: "data/processed/"
  models: "models/"
  reports: "reports/"
  logs: "logs/"

logging:
  level: "INFO" # "DEBUG" for more detail
  file: "logs/inflation_prediction.log"
```

> **INE IDs (quick):** in WSTempus search for “IPC General”, “IPC por grupos”, or “IPCA (armonizado)”, open the series, and use the **numeric ID** at the end of the URL inside `{ID_SERIE_...}`. The template may include default IDs.

## ▶️ Run

### Full pipeline (online)

```bash
python src/main.py
```

Sequence: **download → cleaning → features → models → forecasts → PDF report**.  
Logs in `logs/inflation_prediction.log`.

## 📤 Outputs

In `reports/`:

- `predictions.csv`, `predictions.json` (12 months; with intervals if applicable)
- **Technical PDF**: `informe_tecnico_inflacion_YYYYMMDD_HHMMSS.pdf`
- **Visualizations**:
  - `inflacion_historica_predicciones.png`
  - `comparacion_modelos.png`
  - `distribucion_predicciones.png`
  - `intervalos_confianza.png`
  - `analisis_historico.png`
  - `descomposicion_estacional.png` _(if ≥ 24 months of history)_
- `pipeline_execution_state.json` (execution state)

Additionally:

- `models/`: `arima_model.pkl` (and, if enabled, `random_forest.pkl`, `lstm_model.pkl`)
- `logs/inflation_prediction.log`: execution log

## 📜 Data & attribution

- **Data source:** Instituto Nacional de Estadística (INE) — consumed via **WSTempus**.
- Data usage is governed by INE’s terms.

## 👥 Contributors

- [merygon](https://github.com/merygon)
- [LydiaRuizMartinez](https://github.com/LydiaRuizMartinez)

---

# Sistema de Predicción de Inflación en España

Plataforma end-to-end que **descarga datos del INE**, los **limpia y transforma**, **entrena modelos** (ARIMA; Random Forest y LSTM opcionales), **genera predicciones a 12 meses** e **informa con gráficos y un PDF técnico**.

## **Proyecto dirigido por:** _María González Gómez · Lydia Ruiz Martínez_

## 📌 Qué es y por qué

Solución para **anticipar la inflación en España** con un flujo reproducible y auditable basado en datos oficiales del **Instituto Nacional de Estadística (INE)**. Pensado para **equipos económicos**: ejecutar → obtener **predicciones con intervalos** → recibir **gráficos** e **informe PDF**.

## 🧩 Arquitectura

- `src/main.py` → **Orquesta el pipeline completo** (descarga, limpieza, features, modelos, predicción, reportes).
- `src/ine_extractor.py` → Descarga del INE.
- `src/data_cleaner.py` → Limpieza y normalización de fechas/valores.
- `src/feature_engineering.py` → Variables rezagadas, medias móviles, estacionalidad.
- `src/model_trainer.py` → Entrena ARIMA (base) y opcionalmente RF/LSTM.
- `src/predictor.py` → Predicciones e intervalos.
- `src/report_generator.py` → Gráficos e **informe PDF**.

**Estructura de carpetas**

```
src/                código
data/raw/           CSV del INE descargados (paso 1)
data/processed/     CSV procesados + engineered_features.csv (pasos 2–3)
models/             modelos guardados (p.ej., arima_model.pkl)
reports/            predicciones (csv/json), visualizaciones (png), informe PDF
logs/               logs de ejecución
```

## ✅ Requisitos

- **Python 3.10–3.12**
- Memoria recomendada: **4–8 GB**
- Conexión a internet para descarga INE  
  Dependencias en `requirements.txt` (incluye `statsmodels`, `scikit-learn` y **TensorFlow opcional**).

## ⚙️ Instalación

```bash
# 1) Entorno virtual
python -m venv venv
# Windows
venv\Scripts\Activate.ps1
# macOS/Linux
source venv/bin/activate

# 2) Dependencias
pip install -r requirements.txt
```

## 🗂️ Configuración mínima

Usa **`config/config.yaml`** para fechas, rutas y series del INE. Incluimos **`config/config.example.yaml`** como plantilla: cópiala y ajusta.

```yaml
data:
  start_date: "2015-01-01"
  end_date: "2024-12-31"
  urls:
    ipc_general: "https://servicios.ine.es/wstempus/js/es/VALORES_SERIE/{ID_SERIE_IPC_GENERAL}"
    ipc_grupo: "https://servicios.ine.es/wstempus/js/es/VALORES_SERIE/{ID_SERIE_GRUPO}"
    ipca: "https://servicios.ine.es/wstempus/js/es/VALORES_SERIE/{ID_SERIE_IPCA}"

prediction:
  horizon_months: 12 # horizonte de predicción (meses)

paths:
  data:
    raw: "data/raw/"
    processed: "data/processed/"
  models: "models/"
  reports: "reports/"
  logs: "logs/"

logging:
  level: "INFO" # "DEBUG" para más detalle
  file: "logs/inflation_prediction.log"
```

> **IDs del INE (rápido):** en WSTempus buscad “IPC General”, “IPC por grupos” o “IPCA (armonizado)”, abrid la serie y usad el **ID numérico** al final de la URL dentro de `{ID_SERIE_...}`. La plantilla puede incluir IDs por defecto.

## ▶️ Ejecución

### Pipeline completo (online)

```bash
python src/main.py
```

Secuencia: **descarga → limpieza → features → modelos → predicciones → informe PDF**.  
Logs en `logs/inflation_prediction.log`.

## 📤 Salidas

En `reports/`:

- `predictions.csv`, `predictions.json` (12 meses; con intervalos si aplica)
- **PDF técnico**: `informe_tecnico_inflacion_YYYYMMDD_HHMMSS.pdf`
- **Visualizaciones**:
  - `inflacion_historica_predicciones.png`
  - `comparacion_modelos.png`
  - `distribucion_predicciones.png`
  - `intervalos_confianza.png`
  - `analisis_historico.png`
  - `descomposicion_estacional.png` _(si hay ≥ 24 meses de histórico)_
- `pipeline_execution_state.json` (estado de ejecución)

Además:

- `models/`: `arima_model.pkl` (y, si se activan, `random_forest.pkl`, `lstm_model.pkl`)
- `logs/inflation_prediction.log`: bitácora de ejecución

## 📜 Datos y reconocimiento

- **Fuente de datos:** Instituto Nacional de Estadística (INE) — consumo vía **WSTempus**.
- El uso de datos se rige por las condiciones del INE.

## 👥 Colaboradores

- [merygon](https://github.com/merygon)
- [LydiaRuizMartinez](https://github.com/LydiaRuizMartinez)

---

