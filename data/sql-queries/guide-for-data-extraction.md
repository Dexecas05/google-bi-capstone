## 📄 `DATA_ACQUISITION.md`

### 📊 Obtención del Set de Datos de Análisis (Target Table)

El *dataset* utilizado para el Análisis Exploratorio de Datos (EDA) y las visualizaciones fue construido a partir de **cuatro fuentes de datos** primarias, unidas y agregadas mediante consultas en **Google BigQuery**.

#### 1\. Fuentes de Datos

| Nombre del Dataset | Fuente | Ubicación en BigQuery (o Local) | Descripción |
| :--- | :--- | :--- | :--- |
| **NYC Citi Bike Trips** | Pública | `bigquery-public-data.new_york_citibike.citibike_trips` | Contiene el registro de viajes individuales (la tabla base). |
| **Census Bureau US Boundaries** | Pública | `bigquery-public-data.geo_us_boundaries.zip_codes` | Utilizada para realizar la unión geo-espacial y obtener los códigos postales (ZIP codes) de inicio y fin del viaje. |
| **GSOD Weather Data** | Pública | `bigquery-public-data.noaa_gsod.gsod20*` | Contiene datos diarios de estaciones meteorológicas, unidos por fecha. |
| **NYC ZIP Codes List** | **Local** | `/data/raw/NYC-zip-codes-list.csv` | Listado personalizado de códigos postales de NYC mapeados a **barrio** y **distrito (borough)**. |

-----

### 🚀 Paso Previo Requerido: Carga de Datos Locales

Antes de ejecutar cualquiera de los *scripts* SQL, es **obligatorio** subir el *dataset* local `NYC-zip-codes-list.csv` a BigQuery.

1.  **Cargar el archivo:** Sube el archivo ubicado en `/data/raw/NYC-zip-codes-list.csv` a BigQuery.
2.  **Referencia:** Asegúrate de que este archivo cargado sea accesible y reemplaza la referencia en los *scripts* de la sección **JOIN 4** y **JOIN 5** por la ruta de tu **nuevo *dataset*** (Ejemplo: `project_id.dataset_id.table_id_zip_codes`).

-----

### 📝 Consultas SQL para Generación de Datasets

En el directorio `/data/sql-queries`, se encuentran los siguientes *scripts* que generan los *datasets* finales:

#### A. Consulta 1: Dataset Completo (Target Table - All Year)

Esta consulta agrega los viajes de **2014 y 2015** en intervalos de 10 minutos y une todas las dimensiones (geográficas, temporales y climáticas).

```sql
SELECT
  -- Key Dimensions
  TRI.usertype,
  ZIPSTART.zip_code AS zip_code_start,
  ZIPSTARTNAME.borough AS borough_start,
  ZIPSTARTNAME.neighborhood AS neighborhood_start,
  ZIPEND.zip_code AS zip_code_end,
  ZIPENDNAME.borough AS borough_end,
  ZIPENDNAME.neighborhood AS neighborhood_end,

  -- Temporal Dimensions & Keys (Adjusted for Portfolio)
  DATE_ADD(DATE(TRI.starttime), INTERVAL 5 YEAR) AS start_day,
  DATE_ADD(DATE(TRI.stoptime), INTERVAL 5 YEAR) AS stop_day,
  
  -- Weather Dimensions
  WEA.temp AS day_mean_temperature, -- Mean temp
  WEA.wdsp AS day_mean_wind_speed, -- Mean wind speed
  WEA.prcp AS day_total_precipitation, -- Total precipitation
  
  -- Metrics
  ROUND(CAST(TRI.tripduration / 60 AS INT64), -1) AS trip_minutes,
  COUNT(TRI.bikeid) AS trip_count -- The aggregated measure
FROM
  -- BASE TABLE: Public Citibike Trip Data
  `bigquery-public-data.new_york_citibike.citibike_trips` AS TRI
INNER JOIN
  -- JOIN 1: Geo-Spatial Join for START ZIP Code
  `bigquery-public-data.geo_us_boundaries.zip_codes` AS ZIPSTART
  ON ST_WITHIN(
    ST_GEOGPOINT(TRI.start_station_longitude, TRI.start_station_latitude),
    ZIPSTART.zip_code_geom)
INNER JOIN
  -- JOIN 2: Geo-Spatial Join for END ZIP Code
  `bigquery-public-data.geo_us_boundaries.zip_codes` AS ZIPEND
  ON ST_WITHIN(
    ST_GEOGPOINT(TRI.end_station_longitude, TRI.end_station_latitude),
    ZIPEND.zip_code_geom)
INNER JOIN
  -- JOIN 3: Temporal Join for Daily Weather Data
  `bigquery-public-data.noaa_gsod.gsod20*` AS WEA
  ON PARSE_DATE("%Y%m%d", CONCAT(WEA.year, WEA.mo, WEA.da)) = DATE(TRI.starttime)
INNER JOIN
  -- JOIN 4: Neighborhood Data for START Location (*** REEMPLAZAR CON TU TABLA CARGADA ***)
  `project_id.dataset_id.zip_codes_table` AS ZIPSTARTNAME
  ON ZIPSTART.zip_code = CAST(ZIPSTARTNAME.zip AS STRING)
INNER JOIN
  -- JOIN 5: Neighborhood Data for END Location (*** REEMPLAZAR CON TU TABLA CARGADA ***)
  `project_id.dataset_id.zip_codes_table` AS ZIPENDNAME
  ON ZIPEND.zip_code = CAST(ZIPENDNAME.zip AS STRING)
WHERE
  -- Filter 1: Isolate a single weather station (NEW YORK CENTRAL PARK)
  WEA.wban = '94728'
  -- Filter 2: Limit time frame to 2014 and 2015
  AND EXTRACT(YEAR FROM DATE(TRI.starttime)) BETWEEN 2014 AND 2015
GROUP BY
  1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13
```

#### B. Consulta 2: Dataset Filtrado por Verano (Summer Trend)

Esta consulta es idéntica a la anterior, pero añade un filtro para aislar únicamente los viajes realizados durante los meses de **verano (julio, agosto y septiembre)**.

```sql
SELECT
  -- Key Dimensions
  TRI.usertype,
  ZIPSTART.zip_code AS zip_code_start,
  ZIPSTARTNAME.borough AS borough_start,
  ZIPSTARTNAME.neighborhood AS neighborhood_start,
  ZIPEND.zip_code AS zip_code_end,
  ZIPENDNAME.borough AS borough_end,
  ZIPENDNAME.neighborhood AS neighborhood_end,

  -- Temporal Dimensions & Keys (Adjusted for Portfolio)
  DATE_ADD(DATE(TRI.starttime), INTERVAL 5 YEAR) AS start_day,
  DATE_ADD(DATE(TRI.stoptime), INTERVAL 5 YEAR) AS stop_day,
  
  -- Weather Dimensions
  WEA.temp AS day_mean_temperature, -- Mean temp
  WEA.wdsp AS day_mean_wind_speed, -- Mean wind speed
  WEA.prcp AS day_total_precipitation, -- Total precipitation
  
  -- Metrics
  ROUND(CAST(TRI.tripduration / 60 AS INT64), -1) AS trip_minutes,
  COUNT(TRI.bikeid) AS trip_count -- The aggregated measure
FROM
  -- BASE TABLE: Public Citibike Trip Data
  `bigquery-public-data.new_york_citibike.citibike_trips` AS TRI
INNER JOIN
  -- JOIN 1: Geo-Spatial Join for START ZIP Code
  `bigquery-public-data.geo_us_boundaries.zip_codes` AS ZIPSTART
  ON ST_WITHIN(
    ST_GEOGPOINT(TRI.start_station_longitude, TRI.start_station_latitude),
    ZIPSTART.zip_code_geom)
INNER JOIN
  -- JOIN 2: Geo-Spatial Join for END ZIP Code
  `bigquery-public-data.geo_us_boundaries.zip_codes` AS ZIPEND
  ON ST_WITHIN(
    ST_GEOGPOINT(TRI.end_station_longitude, TRI.end_station_latitude),
    ZIPEND.zip_code_geom)
INNER JOIN
  -- JOIN 3: Temporal Join for Daily Weather Data
  `bigquery-public-data.noaa_gsod.gsod20*` AS WEA
  ON PARSE_DATE("%Y%m%d", CONCAT(WEA.year, WEA.mo, WEA.da)) = DATE(TRI.starttime)
INNER JOIN
  -- JOIN 4: Neighborhood Data for START Location (*** REEMPLAZAR CON TU TABLA CARGADA ***)
  `project_id.dataset_id.zip_codes_table` AS ZIPSTARTNAME
  ON ZIPSTART.zip_code = CAST(ZIPSTARTNAME.zip AS STRING)
INNER JOIN
  -- JOIN 5: Neighborhood Data for END Location (*** REEMPLAZAR CON TU TABLA CARGADA ***)
  `project_id.dataset_id.zip_codes_table` AS ZIPENDNAME
  ON ZIPEND.zip_code = CAST(ZIPENDNAME.zip AS STRING)
WHERE
  -- Filter 1: Isolate a single weather station (NEW YORK CENTRAL PARK)
  WEA.wban = '94728'
  -- Filter 2: Limit time frame to 2014 and 2015
  AND EXTRACT(YEAR FROM DATE(TRI.starttime)) BETWEEN 2014 AND 2015
  -- Filter 3: Limit months to Summer (July, August, September)
  AND EXTRACT(MONTH FROM DATE(TRI.starttime)) IN (7, 8, 9)
GROUP BY
  1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13
```