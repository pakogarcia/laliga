# LaLiga Data Analytics & Predictive Engine

Este repositorio contiene la arquitectura completa de **ETL (Extracción, Transformación y Carga)** y el **Pipeline de Machine Learning** para el análisis predictivo de partidos de la Primera División Española (LaLiga).

El proyecto integra datos estadísticos, meteorológicos y sistemas de ranking Elo para alimentar modelos de IA capaces de predecir resultados (1X2) y mercado de goles.

## 🏗️ Arquitectura del Proyecto

El sistema se divide en tres capas fundamentales:

1. **Capa de Datos (ETL):** Procesamiento de estadísticas, meteorología histórica (Open-Meteo) y cálculo de sistemas de puntuación Elo.
2. **Capa de Modelado (Entrenamiento):** Pipeline de entrenamiento que serializa modelos mediante `joblib` (XGBoost y Random Forest).
3. **Capa de Inferencia (Despliegue):** API REST (FastAPI) y Dashboard interactivo (Streamlit) para predicciones en tiempo real.

## 🛠️ Estructura del Repositorio

```text
/Resultados
├── entrenar_modelos.py     # Pipeline de entrenamiento (IA)
├── api_predicciones.py     # API REST (Servidor)
├── app_web.py              # Dashboard interactivo (Frontend)
├── auditor_equipos.py      # Data Quality del dataset
├── enriquecer_clima.py     # Extracción API meteorológica
├── fusionar_elo_clima.py   # Fusión y cálculo de variables
└── README.md

```

## 🚀 Guía de Ejecución

### 1. Preparación de Datos (ETL)

*Se recomienda activar un entorno e instalar estas librerías*

```
pip install pandas requests openmeteo-requests requests-cache retry matplotlib seaborn scikit-learn xgboost fastapi uvicorn streamlit joblib
```

Para generar el dataset base, sigue el flujo de procesamiento:

1. `python auditor_equipos.py` (Validación de coordenadas).
2. `python enriquecer_clima.py` (Descarga meteorológica masiva).
3. `python fusionar_elo_clima.py` (Cálculo de `dif_elo` y creación del dataset final).

### 2. Entrenamiento y Producción

Una vez generado el dataset `LaLiga_Dataset_Final.csv`:

1. **Entrenar Modelos:** Ejecuta `python entrenar_modelos.py`. Esto creará los archivos `.pkl` necesarios para la predicción.
2. **Activar Servidor (API):** Ejecuta `uvicorn api_predicciones:app`. (Esto levantará el servidor en el puerto 8000).
3. **Lanzar Interfaz Web:** Ejecuta `streamlit run app_web.py`. Accede a `http://localhost:8501` en tu navegador.

## 📋 Requisitos Técnicos

* **Lenguaje:** Python 3.x
* **Librerías principales:** `pandas`, `xgboost`, `scikit-learn`, `fastapi`, `uvicorn`, `streamlit`, `joblib`.

## 📈 Tecnologías Utilizadas

* **ETL:** Pandas (Merge_asof), Open-Meteo API.
* **Modelado:** Random Forest (para goles), XGBoost (para 1X2 multiclase).
* **MLOps:** FastAPI para inferencia, Streamlit para visualización.

## Autor

Proyecto desarrollado por Pako García.

---
