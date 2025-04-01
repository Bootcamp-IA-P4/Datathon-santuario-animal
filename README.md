 # Datathon
Este proyecto es la introducción perfecta al análisis de datos, gráficos y el arte del data storytelling.

-indice-
## Recursos:
* Guía práctica de introducción al análisis exploratorio de datos en Python: [guia_eda_python](https://datos.gob.es/sites/default/files/doc/file/guia_eda_python.pdf)
* (EDA) en Python para Principiantes: Paso a Paso. [Guía de Análisis Exploratorio de Datos](https://dataxpertos.com/guia-analisis-exploratorio-python-eda/)
* GUÍA DE ANALISIS EXPLORATORIO: [TUTORIAL CIENCIA DE DATOS](https://www.youtube.com/watch?v=-nCIBbdDQOg)
*  Cómo elegir el gráfico correcto: [visualizar datos abiertos](https://datos.gob.es/es/blog/como-elegir-el-grafico-correcto-para-visualizar-datos-abiertos)
*  Guía práctica de limpieza de datos: [guía data clean](https://www.datasource.ai/es/data-science-articles/una-guia-practica-para-la-limpieza-de-datos)

## Primeros pasos para realizar un EDA
![EDA](https://i0.wp.com/gravitar.biz/wp-content/uploads/2024/02/8-1.png?resize=752%2C387&ssl=1)

Como puedes observar este proyecto trata sobre el análisis exploratorio de los datos, es el corazón de la ciencia de datos y machine learnig, todo esto lo realizaremos gracias a tres bibliotecas de python que son un éstandar para este análisis y visualización de datos.
* Numpy: se usa para la realización de operaciones númericas debido a su gran eficiencia.
* Pandas: analiza y manipula datos. Comúnmente usada para trabajar con conjunto de datos (dataset) estructurado en filas y columnas (llamado el excel de python).
* Matplotlib: visualizar datos, crea gráficos de forma que los datos quedan mucho más visuales y así podemos comprenderlos mejor. 
## Analizemos nuestro dataset
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

## Exploración Inicial de los Datos
Esto nos dará pistas para ver la estructura, el resumen estadístico y las primeras filas del dataframe. ¿Por qué es importante?, es nuestra primera toma con el dataset con el que vamos a trabajar, nos permite ver los datos al completo ya que el siguiente paso será realizar las preguntas disparadoras.

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

### Preguntas disparadoras
Después de analizar nuestro dataset, debemos definir que finalidad tienen nuestros datos,de todo ello que queremos análizar. Aquí entra en juego las preguntas disipadoras, en mi caso reflexione lo que queria análizar. 

Para esto primero cree un storytelling que me ayudará a darle contexto a estos datos:

> El Santuario Nueva Amazonas ha registrado un alarmante incremento de una enfermedad viral letal que afecta a diversas especies. Ante la urgencia de contener su propagación, se nos ha encomendado la tarea de analizar minuciosamente los datos clínicos para identificar patrones en los síntomas. Con esta información, el equipo médico podrá diseñar estrategias precisas para erradicar el virus y, así, preservar la rica diversidad biológica que habita.

Con este contexto y gracias a la visualización previa podemos reflexionar que datos nos parecen relevantes analizar para poder obtener un mejor resultado. Como experta en salud animal mis preguntas disparadoras son:

> **¿Qué proporción de casos son considerados peligrosos vs no peligrosos?**
> 
> **Cuáles son los síntomas más frecuentes?**
> 
> **¿Cuáles son las especies que más sufren estos sintomas?**
> 
> **¿Cuáles son las especies que mas sufren el sintoma principal?**
> 
> **¿Cúales son las especies que más han fallecido?**
> 
> **¿Cúales son el conjunto de síntomas presentados por animales fallecidos?**

Con esto claro vamos con el siguiente paso:

## Limpieza de datos

Es fundamental antes de ponernos a graficar, no tiene sentido trabajar en un entorno sucio, pasa lo mismo con los datos, de que nos sirve gráficar los síntomas mas frecuentes si en varias lineas ese dato esta en blanco o hay alguna incongruencia.

En este caso el dataset venia bastante limpio (algo que como novata agradezco) solo tenia que limpiar dos valores nulos en la columna "Dangerous".

Para esta limpieza usaremos los siguientes métodos:
```python
# Revisamos la cantidad de valores nulos en cada columna
print(df.isnull().sum())
```
Una vez revisadon donde esta el error procedemos a arreglarlo
```python
# Rellenamos los valores nulos en la columna 'Dangerous' con 'No'
df['Dangerous'] = df['Dangerous'].fillna('No')

# Creamos una columna numérica para Dangerous: Yes -> 1, No -> 0
df['Dangerous_numeric'] = df['Dangerous'].map({'Yes': 1, 'No': 0})

# Revisamos nuevamente los valores únicos
print("\nValores únicos en Dangerous:", df['Dangerous'].unique())
```
Cómo mi dataset solo tenia este pequeño fallo, de momento mi parte de limpieza esta lista pero te dejo otros métodos por si hay que realizar una limpieza más exhaustiva:

| Paso | Descripción | Código |
| ------------- | ------------- | ------------- |
| Eliminación de filas y columnas redundantes | Se eliminan las tres primeras filas que contienen información repetida y las columnas de promedios innecesarios. | `df.drop([0, 1, 2], axis=0, inplace=True)`<br>`df.reset_index(drop=True, inplace=True)`<br>`df.drop(df.columns[1::3], axis=1, inplace=True)` |
| Extracción del valor numérico | Se extrae el valor numérico (primer elemento) de cada celda que incluye un valor y un rango. | `for i in range(1, 85):`<br>`&nbsp;&nbsp;df.iloc[:, i] = df.iloc[:, i].str.split(expand=True)[0]` |
| Transformación a formato largo | Se convierte el DataFrame de formato ancho a uno largo usando `melt`. | `df2 = df.melt(id_vars=['Unnamed: 0'], value_name='obesity_rate')` |
| Separación de columnas (año y género) | Se separa la columna que codifica año y género en dos columnas: `year` y `gender`, y se elimina la columna original. | `df2[['year', 'gender']] = df2.iloc[:,1].str.split('.', expand=True)`<br>`df2.drop('variable', axis=1, inplace=True)` |
| Renombrado y mapeo de género | Se renombra la columna de país y se reemplazan los códigos de género numéricos por etiquetas (`male`/`female`). | `df2.rename(columns={'Unnamed: 0': 'country'}, inplace=True)`<br>`gender = {'1': 'male', '2': 'female'}`<br>`df2.gender.replace(gender, inplace=True)` |
| Eliminación de filas con valores "No" | Se eliminan las filas correspondientes a países que tienen el valor `"No"` en `obesity_rate`. | `omit = df2[df2.obesity_rate == 'No']['country'].unique()`<br>`df2 = df2[~df2.country.isin(omit)]` |
| Conversión de tipo numérico | Se convierte la columna `obesity_rate` a tipo `float32` para facilitar el análisis. | `df2 = df2.astype({'obesity_rate': 'float32'})` |
| Revisión final del DataFrame | Se revisa la forma, los valores nulos y los tipos de datos del DataFrame final. | `df2.shape`<br>`df2.isna().sum()`<br>`df2.dtypes` |


## Visualización de Datos
La visualización de datos es esencial en el análisis exploratorio, ya que no solo complementa los análisis numéricos, sino que también permite identificar patrones, tendencias, relaciones entre variables y anomalías que podrían pasar desapercibidas. Para ello, utilizaremos diversas herramientas gráficas como histogramas, gráficos de líneas, diagramas de barras y gráficos de sectores—adaptadas a distintos tipos de análisis. En particular, el histograma es muy útil para representar la distribución de variables numéricas al agrupar los datos en intervalos, lo que facilita detectar características estadísticas fundamentales como la tendencia central, la variabilidad, la presencia de valores extremos, discontinuidades y posibles valores atípicos.

He de confesar que esta ha sido mi parte favorita para desarrollar el EDA, soy una persona muy visual y me encanto ver las gráficas, a pesar de que tuve algunos problemas ya que mis datos se veian tal que asi:
![datos mal gestionados](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/Captura%20de%20pantalla%202025-03-31%20232408.png)

Quede bastante horrorizada con esto sin embargo era por que no me percate que habia demasiados síntomas distintos (muchas categorías únicas), de modo que cada categoría se convertia en una barra individual y el gráfico se vuelve ilegible. ¿Cómo lo solucione? Pues reduciendo las categorías a las que más se repiten, creando un top 20 síntomas más frecuentes. Así evite que el gráfico se sature con datos poco relevantes.

![analisis de sintomas](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/An%C3%A1lisis%20de%20los%20s%C3%ADntomas.png)

Esto esta mucho mejor, ¿Qué cuenta este dato?
Me muestra los 20 sintomas que mas se repinten, con este dato ayudamos a los médicos veterinarios a estar alerta de esos animales que empiecen a presentarlos ya que es bastante probable que esten incubando el virus, la actuación temprana ante esta situación es fundamental para garantizar el bienenstar de estos animales.

----

También use un diágrama de pastel ya que son datos que contrastan. Esta gráfica informa el rango de casos clínicos registrados como peligrosos ya que el riesgo tasa de mortalidad es elevada.

![peligroso](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/Proporci%C3%B3n%20de%20casos%20peligrosos%20vs%20no%20peligrosos.png)

----

Esta gráfica les ayuda a los veterinarios a conocer que animales deben tenerlos bajo vigilancia, eso hace que su trabajo sea menos fragmentado en esta crisis virica.
![animales con afecciones](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/Cu%C3%A1l%20es%20la%20especie%20que%20m%C3%A1s%20sufre%20esos%20s%C3%ADntomas.png)

---
En este caso tener fiebre, parece ser un indicativo clave y que se repite con frecuencia en los casos sobretodo peligrosos, conocer este dato no solo refuerza la rapida ejecución, si no también conocer un top de especies puede permitir realizar mejores pruebas de diagnostico para así conseguir erradicar este virus.

![animales con fiebre](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/Cu%C3%A1les%20son%20las%20especies%20que%20mas%20sufren%20el%20sintoma%20principal.png)

----

¿Cuáles son las especies que han falllecido debido a este virus? 
Para ello vamos a filtrar los datos que tengan la etiqueta death, y mostrarlo en en un gráfico sencillo de lineas horizontales, esto ayudará a los médicos veterinarios a conocer que especie tiene una baja resistencia y reforzar sus defenzas en un futuro brote.

![animales fallecidos](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/Cu%C3%A1les%20son%20las%20especies%20que%20han%20falllecido%20debido%20a%20este%20virus.png)

----

Esta gráfica es clave ya que nos da un patrón de cuales han sido los síntomas que han acompañado a esos animales que tenian la etiqueta "death", esto ayuda a determinar el protocolo de actuación ya que con esto se confirman cuales son la mayoria de los sintomas letales que ocaciona el virus, si se actua de una forma rápida la tasa de mortalidad se reducirá.

![sintomas cons etiqueta death](https://github.com/abbyenredes/Datathon/blob/docs/readme/img/C%C3%BAales%20son%20el%20conjunto%20de%20s%C3%ADntomas%20presentados%20por%20animales%20fallecidos.png)

----

##  Recomendaciones y Próximos Pasos
Focalizar Análisis en las Especies más Afectadas
 * Investigar a fondo las 4 especies con mayores índices de mortalidad, generando perfiles sintomáticos específicos.

Evaluar la Influencia de “Uteria Inertia”
 * Profundizar en casos de hembras afectadas y analizar si esta afección incrementa la letalidad cuando se combina con el virus.

Extender el Análisis de Síntomas
 * Revisar los síntomas de menor frecuencia pero presentes en el top 20, para detectar posibles asociaciones aún no evidentes.

Mejorar Protocolos de Detección Temprana
 * Proponer herramientas de cribado enfocado en síntomas críticos (p.ej., combinaciones fiebre + diarrea + tos), evaluando su eficacia para acciones preventivas.

Validar Estrategias de Intervención
 * Colaborar con el equipo clínico para monitorizar el impacto de cualquier medida implementada y ajustar la estrategia en función de resultados reales.


