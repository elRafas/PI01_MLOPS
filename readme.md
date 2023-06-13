# **PI MLOPs**

# Introduccion 

Éste es mi proyecto individual de Ciencia de Datos para **Soy Henry**.

El proyecto trata de un dataset de películas con información acerca del título, el género, el año, una calificacion, popularidad, etc. En donde responde a una serie de peticiones en una API. También se dispone de un modelo de recomendación de dichas películas.


# Objetivo
El objetivo principal es desarrollar un sistema de recomendación basado en contenido (content-based) a partir del dataset, que junto con unas serie de endpoints para realizar el Deploy. Este objetivo se divide en varios pasos:

1. Limpiar y Transformar: Realizar un proceso ETL (Extract-Transform-Load) para limpiar el dataset.
2. Análisis Exploratorio de Datos (EDA): Analizar posibles relaciones o palabras que ayuden al sistema.
3. Desarrollar una API: Desarrollar 6 funciones para consultar datos de las películas y para poder consumir el modelo.
4. Sistema de recomendacion: Será un sistema basado en el contenido del producto, 
5. Deploy: Dejar consumible la API para acceder desde internet.

# Funciones API
El desarrollo de la API se realizó con FastAPI.
Cada funcion se ejecuta sobre un dataset posterior al proceso ETL, el cual se redujo sólo a los datos necesarios para las funciones.
el endpoint para consumir el modelo ejecuta a partir de un set de datos exclusivo y con una cantidad de tuplas reducido, para poder ser ejecutado a través del servicio.

Los inputs tienen cierta tolerancia con las mayúsculas pero deben estar bien escritos.
[endpoints](/endpoints.ipynb)


# ETL
Se procesó el set de películas y se limpiaron los valores faltantes.
1. Desanidar:
Existen columnas en el dataset original que contienen strings con estructura json. entonces lo primero es extraer esa información y convertir cada columna en un dataset individual, para finalmente devolverlo al dataset original en una estructura sencilla de tratar en la API.
2. Valores Nulos:
Las columnas numéricas nulas (NaN) fueron reemplazados por 0.
Se eliminaron registros en la columna 'release_date'.
3. Nuevas columnas:
los datos de 'release_year' fueron extraidos por la columna 'release_date'.
Se agregó una nueva columna 'return', el retorno de inversión.
4. Columnas innecesarias:
Se eliminaron columnas innecesarias

# Sistema de Recomendación
## Preprocesamiento
Se utilizó un modelo de recomendación content-based en donde se analizan las palabras, y se comparan la cantidad de veces que se repiten.
Este sistema toma los ciertos valores en el dataset post-ETL:

1. overvew. resumen de la película.
2. genres. géneros que definen la película.

Se tomaron los 5000 registros mejor puntuados.

Los registros pasan por cierto filtro para estandarizar las palabras, como pasar a minúsculas, o eliminar caracteres especiales. Se condensan las palabras en un sólo atributo llamado 'tags'.
## Modelo
Las palabras y sus recuentos fueron procesados por el algoritmo similitud del coseno.

# Deploy
Para disponibilizar lo desarrollado, se utilizó el servicio gratuito de Render.

A continuación el link de la [API](https://elrafas-pi01-ml-ops.onrender.com).
