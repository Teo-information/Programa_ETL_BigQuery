# Laboratorio_4

## Descripción General

**Laboratorio_4** es un cuaderno Jupyter orientado a la gestión y actualización de datos de un modelo dimensional para un sistema de ventas, utilizando Python, pandas y Google BigQuery. El flujo abarca desde la ingestión de archivos CSV, la integración de nuevos registros, la exportación de datos actualizados y la carga automatizada a BigQuery, siguiendo buenas prácticas de ETL (Extract, Transform, Load).

---

## Estructura del Notebook

### 1. Ingesta de Datos

Se realiza la carga de archivos CSV correspondientes a las tablas principales del modelo dimensional:

- DimCategoria
- DimCliente
- DimEmpleado
- DimFP (Forma de Pago)
- DimMarca
- DimMes
- DimProducto
- DimSubCategoria
- DimTiempo
- DimVentas (tabla de hechos)

```python
import pandas as pd
df = pd.read_csv('DimCategoria.csv', sep=';')
# ...similar para el resto de tablas...
```

---

### 2. Integración de Nuevos Registros

Se generan nuevos DataFrames con registros adicionales para cada tabla, que luego se concatenan con los datos originales para simular la actualización de dimensiones y hechos.

```python
new_data = {'id_cat,nom_cat': ['5,Panadería', ...]}
new_df = pd.DataFrame(new_data)
df = pd.concat([df, new_df], ignore_index=True)
```

---

### 3. Exportación de Datos Actualizados

Los DataFrames resultantes se exportan a archivos CSV actualizados, listos para ser utilizados en procesos posteriores o para su carga en sistemas externos.

```python
df.to_csv('DimCategoria_Actualizado.csv', index=False, sep=';')
# ...similar para el resto de tablas...
```

---

### 4. Integración con Google Drive

Se monta Google Drive para facilitar la lectura y escritura de archivos en el entorno de Google Colab.

```python
from google.colab import drive
drive.mount('/content/drive')
```

---

### 5. Carga de Datos a Google BigQuery

Se autentica el usuario y se utiliza la librería `google.cloud.bigquery` para cargar los datos actualizados a tablas en un dataset de BigQuery. El proceso incluye validaciones de estado y consistencia de filas.

```python
from google.cloud import bigquery
client = bigquery.Client(project='your_project_id')
job_config = bigquery.LoadJobConfig(write_disposition="WRITE_TRUNCATE")
job = client.load_table_from_dataframe(dataframe, table_id, job_config=job_config)
job.result()
```

Se verifica el estado de la carga y la cantidad de filas cargadas respecto al DataFrame original.

---

## Requisitos

- Python 3.x
- pandas
- Google Colab (recomendado)
- Acceso a Google Drive
- Acceso a un proyecto de Google Cloud con BigQuery habilitado

---

## Ejecución

1. Subir los archivos CSV originales al entorno de trabajo o Google Drive.
2. Ejecutar cada celda del notebook en orden.
3. Verificar los resultados de la carga en BigQuery y la consistencia de los datos.

---

## Referencias

- [Documentación de pandas](https://pandas.pydata.org/docs/)
- [Google Colab](https://colab.research.google.com/)
- [Google BigQuery Python Client](https://cloud.google.com/python/docs/reference/bigquery/latest)
- [Carga de datos a BigQuery desde pandas DataFrame](https://cloud.google.com/bigquery/docs/loading-data-cloud-storage-pandas)

---

**Nota:** Este laboratorio está diseñado para prácticas de integración y automatización de cargas de datos en la nube, siguiendo estándares técnicos y