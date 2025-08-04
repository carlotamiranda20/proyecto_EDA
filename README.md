# proyecto_EDA

1-Carga de archivos.
En primera lugar cargamos los archivos con los que tenemos que trabajar: 
- usamos pd.read_csv("bank-additional.csv"), que carga el archivo .csv en un dataset , contiene información de clientes y llamadas telefónicas de campañas de marketing bancarias.
- con pd.ExcelFile("customer-details.xlsx") leemos y almacenmos el archivo de Excel. Este archivo tiene varias hojas (sheets), cada una con datos de clientes de distintos años. Con  pd.concat() unimos las 3 hojas de excel en una sola, para tener una sola tabla de datos.

2- Visualizamos los archivos.
Lo siguiente que hacemos es ver los datos de los archivos, qué forma tienen, qué información contienen exactamente. Pra ello usamos diferentes funciones predefinidas: 
- print(df_bank.shape)  -> nos muestra el número de filas y columnas que tiene el archiv
- print(df_bank.columns)   -> muestra los nombres de las columnas, es decir las diferentes variables que hay
- print(df_bank.info())  ->proporciona inforamción sobre el nombre de la columna, el tipo de datos que contienen y el número de valores que faltan (nulls)  . Es útil para conocer qué datos faltan, y si están registrados en un formato que no es el que corresponde, por ejemplo números o fechas como strings. 
- print(df_bank.describe()) -> Estadísticas para los valores numéricos, commo la media, mínmo, máximo, percentiles, desviación estándar.
- print(df_bank.head())  -> muestra las 5 primeras filas del archivo de datos, para ver cómo están escritos y estructurados los datos realmente.
Después de este análisis podemos tener una idea de cuántas filas y columnas hay en cada dataset, los nombres de variables y su significado, detectar posibles columnas con datos faltantes, valores raros como "unknown", o tipos que hay que cambiar.

Estás lista para pasar al Paso 2: limpieza y transformación
