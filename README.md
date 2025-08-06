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



3-Vamos a limpiar y tranformar los datos: corregir datos faltantes o incorrectos, arreglar tipos de datos, crear columnas nuevas.

En primer lugar vamos a arreglar los tipos de datos de las columnas para poder trabajar más cómodamente con ellos. Vamos a realizar los siguiente: 
- Hay muchas columnas tipo object que sson numéricas pero con formato incorrecto (cons.price.idx', 'cons.conf.idx', 'euribor3m', 'nr.employed') tienen valores con comas en vez de puntos decimales, vamos a convertir a float y cambiar comas por puntos.
- Columnas con valores binarios (default, housing, loan) están en tipo float64 (0.0 y 1.0). Vamos a convertirlas a enteros.



En primer lugar vamos a ver los valores nulos que hay en cada columna, con df_bank.isnull().sum(). La función isnull() valora si hay un valor nulo o no en cada fila y con sum() sumamos el total de cada columna con las filas. En el archivo de customers no encontramos ningún valor nulo, mientras que en bank sí. Notar que antes mostramos el número de filas que tenía cada variable (o sea número de filas de cada columna) y no todas tenian 43000 que sería el total. En este caso debe coincidir el número de los valores que nos salen nulos con la diferencia del total con los valores no nulos que obtuvimos en el apartado anterior. 
Apreciamos que en el archivo bank-additional sale un número considerable de valores nulos. 
Vamos a calcular el porcentaje de filas nulas que hay en cada columna resepecto al total, dividiendo el número de valores nulos entre el total de filas. 
Vamos a rellenar los valores nulos de la siguiente forma:  los valores numéricos rellenamos con la mediana (age, cons.price,idx, y euribor3m) , los valores binarios con la moda, es decir el más frecuente (education, loan, housing). La columna default nos indica el historial de impagos, y vemos que tiene un alto porcentaje de valores nulos (20.89% , es decir no hay información de 1 de cada 5 registros). También observamos que la mayoría de los valores es 0 (que no ha habido impagos) y no ofrece mucha variabilidad. Así, hay un alto porcentaje de valores nulos y poca variablidad, por tanto es una columna que no nos aporta mucha información y podemos considerar eliminar la columna ya que no nos aporta realmente información al análisis. 
Depués de hacer esta limpieza y las imputaciones, vemos que en las columnas job, marital y date sigue habiendo valores nulos, pero suponen un porcentaje muy pequeño que no supone un problema en el análisis
