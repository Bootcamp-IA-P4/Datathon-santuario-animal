# Datathon
Aquí pon una descripcion atractiva de tu resultado
-indice-
## Recursos:
* Guía práctica de introducción al análisis exploratorio de datos en Python: [guia_eda_python](https://datos.gob.es/sites/default/files/doc/file/guia_eda_python.pdf)
* (EDA) en Python para Principiantes: Paso a Paso. [Guía de Análisis Exploratorio de Datos](https://dataxpertos.com/guia-analisis-exploratorio-python-eda/)
* GUÍA DE ANALISIS EXPLORATORIO: [TUTORIAL CIENCIA DE DATOS](https://www.youtube.com/watch?v=-nCIBbdDQOg)
*  Cómo elegir el gráfico correcto: [visualizar datos abiertos](https://datos.gob.es/es/blog/como-elegir-el-grafico-correcto-para-visualizar-datos-abiertos)

## Primeros pasos para realizar un EDA
![EDA](https://i0.wp.com/gravitar.biz/wp-content/uploads/2024/02/8-1.png?resize=752%2C387&ssl=1)

Como puedes observar este proyecto trata sobre el análisis exploratorio de los datos, es el corazón de la ciencia de datos y machine learnig, todo esto lo realizaremos gracias a tres bibliotecas de python que son un éstandar para este análisis y visualización de datos.
* Numpy: se usa para la realización de operaciones númericas debido a su gran eficiencia.
* Pandas: analiza y manipula datos. Comúnmente usada para trabajar con conjunto de datos (dataset) estructurado en filas y columnas (llamado el excel de python).
* Matplotlib: visualizar datos, crea gráficos de forma que los datos quedan mucho más visuales y así podemos comprenderlos mejor. 
## Analizemos un dataset
Cosas a tener en cuenta:
1. Realizar un análisis descriptivo de las variables para obtener una idea representativa del conjunto de
datos.
2. Reajustar los tipos de las variables para que sean consistentes al momento de realizar posteriores
operaciones.
3. Detectar y tratar los datos ausentes para poder procesar adecuadamente las variables numéricas. Los
datos ausentes son valores no registrados en algunas observaciones, y es esencial gestionarlos
correctamente para evitar sesgos y problemas en el análisis.
4. Identificar datos atípicos y su tratamiento para evitar que puedan distorsionar futuros análisis
estadísticos.
5. Realizar un examen numérico y gráfico de las relaciones entre las variables analizadas para determinar
el grado de correlación entre ellas, pudiendo predecir el comportamiento de una variable en función de
las otras.

¿Cómo ponemos todo esto en práctica?
Para ello primero importaremos las bibliotecas antes mencionadas con sus correspondientes alias:

```python
# Cargar las librerías necesarias
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```
Después vamos a cargar los datos de un dataframe yo usare este [animal-condition-analysis](https://www.kaggle.com/code/francisoharaaidoo/hth-2023-animal-condition-analysis)

> [!NOTE]
> Como herramienta para este caso práctico, se ha utilizado el lenguaje de programación Python y el entorno
de desarrollo Jupyter Notebook en Google Colab

### Exploración Inicial de los Datos

```python
# Cargar los datos en un DataFrame
df = pd.read_csv('animal_condition.csv')
print(df.head()) # Mostrar las primeras filas del DataFrame
print("="*100) #esto es un separador visual

# Mostrar la estructura del DataFrame
print(df.info()) # Obtener información sobre los tipos de datos y la cantidad de valores no nulos
print("="*100)

# Mostrar un resumen estadístico de las variables numéricas
print(df.describe(include= 'all')) # Estadísticas descriptivas de las variables numéricas
print("="*100)
```

> [!IMPORTANT]
> df.head(): muestra las primeras filas del DataFrame.
> 
>  df.info(): proporciona una vista compacta de la estructura interna del DataFrame, indicando los
> tipos de variables, el número de valores no nulos y la memoria utilizada.
> 
> df.describe(): muestra un resumen estadístico de las variables numéricas del DataFrame,
> incluyendo mínimo, máximo, media, mediana, primer y tercer cuartil, y el número de valores
> faltantes.

### Limpieza de datos


### Visualización de Datos Básica
La visualización de datos es esencial en el análisis exploratorio, ya que no solo complementa los análisis numéricos, sino que también permite identificar patrones, tendencias, relaciones entre variables y anomalías que podrían pasar desapercibidas. Para ello, se utilizan diversas herramientas gráficas—como histogramas, gráficos de líneas, diagramas de barras y gráficos de sectores—adaptadas a distintos tipos de análisis. En particular, el histograma es muy útil para representar la distribución de variables numéricas al agrupar los datos en intervalos, lo que facilita detectar características estadísticas fundamentales como la tendencia central, la variabilidad, la presencia de valores extremos, discontinuidades y posibles valores atípicos.

En este caso vamos a usar:

