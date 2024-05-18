# Análisis de Datos de Ingresos y Salarios de la NBA

![Python Logo](https://github.com/MontielAguilar/47-Data-clean/blob/main/python-logo.png)


## Descripción del Proyecto
Este proyecto analiza los datos históricos de ingresos per cápita ajustados por inflación de varios países y los datos de salarios de jugadores de la NBA de la temporada 2017-2018. El análisis incluye la manipulación de datos, limpieza, recodificación y algunos cálculos estadísticos básicos.

## Datos Utilizados

### Ingresos Per Cápita
El dataset de ingresos per cápita ajustados por inflación se obtuvo de [este enlace](https://raw.githubusercontent.com/JJTorresDS/ds-data-sources/main/income_per_person_gdppercapita_ppp_inflation_adjusted.csv). Contiene datos desde el año 1799 hasta el 2049.

### Salarios de la NBA
El dataset de salarios de jugadores de la NBA proviene de un archivo SQLite (`nba_salary.sqlite`) que contiene dos tablas:
- `NBA_season1718_salary`: Salarios de los jugadores en la temporada 2017-2018.
- `Seasons_Stats`: Estadísticas de jugadores desde 1950 hasta 2017.

## Estructura del Proyecto

### Parte 1: Análisis de Ingresos Per Cápita

1. **Lectura del Dataset:**
    ```python
    import pandas as pd

    url = 'https://raw.githubusercontent.com/JJTorresDS/ds-data-sources/main/income_per_person_gdppercapita_ppp_inflation_adjusted.csv'
    df = pd.read_csv(url, sep=',')
    ```

2. **Verificación de Datos Nulos y Duplicados:**
    ```python
    sum(df.isnull().sum())
    df = df.drop_duplicates()
    ```

3. **Análisis de Ingresos de Argentina (2017-2023):**
    ```python
    df_pais = df[df['country'] == 'Argentina'].iloc[:, 217:224]
    lista = df_pais.values[0]
    lista_nueva = [float(string.replace("k", "")) for string in lista]
    import statistics
    promedio = statistics.mean(lista_nueva) * 1000
    ```

4. **Análisis Comparativo de Países Sudamericanos:**
    ```python
    df_paises = df[df['country'].isin(['Argentina', 'Chile', 'Colombia', 'Bolivia', 'Peru', 'Brasil', 'Uruguay', 'Venezuela', 'Paraguay', 'Ecuador'])].iloc[:, 217:224]
    lista = df_paises.values
    lista_nueva = [float(j.replace("k", "")) * 1000 if 'k' in j else float(j) for string in lista for j in string]
    promedio = statistics.mean(lista_nueva)
    ```

5. **Análisis Comparativo de Países Norteamericanos:**
    ```python
    df_paises = df[df['country'].isin(['United States', 'Canada', 'Mexico', 'Costa Rica', 'Nicaragua'])].iloc[:, 217:224]
    lista = df_paises.values
    lista_nueva = [float(j.replace("k", "")) * 1000 if 'k' in j else float(j) for string in lista for j in string]
    promedio = statistics.mean(lista_nueva)
    ```

### Parte 2: Análisis de Salarios de la NBA

1. **Lectura de Datos desde SQLite:**
    ```python
    import sqlite3

    con = sqlite3.connect("nba_salary.sqlite")
    df = pd.read_sql_query('SELECT * FROM NBA_season1718_salary', con)
    df1 = pd.read_sql_query('SELECT * FROM Seasons_Stats', con)
    con.close()
    ```

2. **Verificación de Datos Nulos:**
    ```python
    df.isnull().sum()
    df1.isnull().sum()
    ```

3. **Limpieza de Datos de Estadísticas:**
    ```python
    percent_missing = df1.isnull().sum() * 100 / len(df1)
    missing_value_df = pd.DataFrame({'Columnas': df1.columns, 'Porcentaje_Missing': percent_missing})
    missing_value_df_f = missing_value_df[missing_value_df['Porcentaje_Missing'] <= 50]
    lista_variables = list(missing_value_df_f.Columnas)
    df1_final = df1[lista_variables]
    ```

## Instrucciones de Uso

1. **Clonar el repositorio:**
    ```bash
    git clone https://github.com/tu_usuario/tu_repositorio.git
    ```

2. **Instalar dependencias:**
    ```bash
    pip install -r requirements.txt
    ```

3. **Ejecutar el análisis:**
    Abre el notebook de Jupyter y ejecuta las celdas correspondientes.

## Contribuciones
Las contribuciones son bienvenidas. Por favor, realiza un fork del proyecto y envía un pull request con tus cambios.

