
# LaLiga Data Analytics

Este repositorio contiene la arquitectura de extracción, transformación y carga (ETL) de datos para un Trabajo de Fin de Máster centrado en el análisis predictivo de partidos de fútbol de la Primera División Española (LaLiga).

El proyecto cruza tres fuentes de información fundamentales para el análisis de Machine Learning:
1. **Estadísticas de Juego y Cuotas:** Histórico de partidos y valor de mercado (Bet365 & Mercado Medio).
2. **Meteorología Histórica:** Temperatura, lluvia y viento el día exacto de cada partido extraído vía API (Open-Meteo).
3. **Sistema Elo:** Puntuación histórica del estado de forma real de los clubes.

---

## Requisitos e Instalación

Para ejecutar este proyecto, es necesario tener instalado Python 3.x. Se recomienda encarecidamente utilizar un entorno virtual.

1. **Crear y activar el entorno virtual:**
  ```bash
  python -m venv env_futbol
  env_futbol\Scripts\activate

```

2. **Instalar las dependencias:**
```bash
pip install pandas requests openmeteo-requests requests-cache retry

```



---

## Estructura del Proyecto

* `SP1_total.csv`: Dataset original limpio con el histórico de partidos y cuotas (formato punto y coma `;`).
* `EloSP1.csv`: Historial de puntuaciones Elo de los equipos de LaLiga.
* `coordenadas_equipos.csv`: Diccionario geográfico con la Latitud y Longitud de los estadios (necesario para el clima).
* `auditor_equipos.py`: Script de *Data Quality* que verifica que todos los equipos históricos tengan sus coordenadas registradas antes del procesamiento.
* `enriquecer_clima.py`: Script principal de extracción meteorológica. Incluye lógica de guardado continuo para reanudar la descarga en caso de superar el límite de la API (soporte para cambio de IP/VPN).
* `fusionar_elo_clima.py`: Script de unión final que cruza el dataset meteorológico con el archivo Elo, calculando la Diferencia de Elo (`dif_elo`) para el modelado.

---

## Flujo de Ejecución (Pipeline)

Para reproducir el dataset final desde cero, sigue este orden estricto:

### Paso 1: Auditoría de Datos

Ejecuta el auditor para asegurar que no falta ningún equipo en el diccionario de coordenadas:

```bash
python auditor_equipos.py

```

*(Si falta algún equipo, añádelo manualmente a `coordenadas_equipos.csv` y vuelve a ejecutar).*

### Paso 2: Descarga Meteorológica

Inicia la consulta masiva a la API de Open-Meteo:

```bash
python enriquecer_clima.py

```

> **⚠️ Nota sobre límites de API:** Este script está diseñado para realizar guardados automáticos. Si la API bloquea el acceso por exceso de peticiones (Error 429), el script se detendrá de forma segura. Simplemente cambia de IP usando una VPN y vuelve a lanzar el mismo comando. El código saltará los partidos ya procesados y continuará donde lo dejó. Generará el archivo temporal `SP1_con_clima.csv`.

### Paso 3: Fusión con Elo (Dataset Final)

Una vez terminado el proceso meteorológico, cruza los datos con el rendimiento deportivo:

```bash
python fusionar_elo_clima.py

```

### Resultado Final

Se generará el archivo definitivo **`LaLiga_Dataset_Final.csv`** (separado por comas `,`), listo para ser importado en cuadernos de Jupyter (`.ipynb`) para el entrenamiento de modelos predictivos.

---

## Autor

*Proyecto desarrollado por Pako García (Análisis de Datos).*

