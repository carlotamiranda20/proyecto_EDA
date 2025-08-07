# Proyecto_EDA
En el proyecto está estructurado de la siguiente forma:
1-Tratamiento de los datos de los archivos: carga de los archivos, visualización, limpieza y transformación de los datos.
2-Análisis de los datos: representaciones gráficas de los archivos donde se comparas y visualizan las diferentes variables y relaciones entre ellas para obtener un análisis  sobre ellos de forma cualitativa.



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

En primer lugar vamos a arreglar los tipos de datos de las columnas para poder trabajar mejor con ellos. Vamos a realizar los siguiente: 
- Hay muchas columnas tipo object que sson numéricas pero con formato incorrecto (cons.price.idx', 'cons.conf.idx', 'euribor3m', 'nr.employed') tienen valores con comas en vez de puntos decimales, vamos a convertir a float y cambiar comas por puntos.
- Columnas con valores binarios (default, housing, loan) están en tipo float64 (0.0 y 1.0). Vamos a convertirlas a enteros (int8)
- La columna 'date' es objeto, pero es fecha en formato texto. Como está en español vamos a dejarlo en ese formato ya que si lo convertimos a datetime no lo va a reconocer como tal.
- Las variables categóricas (job, marital, education, contact, poutcome, y) están en objeto, correcto, pero las convertimos a category para optimizar memoria y análisis.

Lo siguiente que hacemos es ver los valores nulos que hay en cada columna, con df_bank.isnull().sum(). La función isnull() valora si hay un valor nulo o no en cada fila y con sum() sumamos el total de cada columna con las filas. En el archivo de customers no encontramos ningún valor nulo, mientras que en bank sí. Notar que antes mostramos el número de filas que tenía cada variable (o sea número de filas de cada columna) y no todas tenian 43000 que sería el total. En este caso debe coincidir el número de los valores que nos salen nulos con la diferencia del total con los valores no nulos que obtuvimos al aplicar la función info().

En el archivo bank-additional sale un número considerable de valores nulos. Vamos a omitir el hecho de que salgan nulos los valores de date, ya que es porque no entiende los meses en español y al transformarlo a tipo datetime detecta los valores como nulos.

Vamos a calcular el porcentaje de filas nulas que hay en cada columna resepecto al total, dividiendo el número de valores nulos entre el total de filas. 

Vamos a rellenar los valores nulos de la siguiente forma:  los valores numéricos rellenamos con la mediana (age, cons.price,idx, y euribor3m) , los valores binarios con la moda, es decir el más frecuente (education, loan, housing). La columna default nos indica el historial de impagos, y vemos que tiene un alto porcentaje de valores nulos (20.89% , es decir no hay información de 1 de cada 5 registros). También observamos que la mayoría de los valores es 0 (que no ha habido impagos) y no ofrece mucha variabilidad. Así, hay un alto porcentaje de valores nulos y poca variablidad, por tanto es una columna que no nos aporta mucha información y podemos considerar eliminar la columna ya que no nos aporta realmente información al análisis. 

Depués de hacer esta limpieza y las imputaciones, vemos que en las columnas job, marital y date sigue habiendo valores nulos, pero suponen un porcentaje muy pequeño que no supone un problema en el análisis.

Lo siguiente que hacemos es mirar si hay filas duplicadas que no nos aporten información. Usamos: num_duplicados = df_bank.duplicated().sum(). Vemos que no hay filas duplicadas.

4- Con este estudio y transformación de los datos hecho, pasamos a la parte de visualización y análisis. 
Comenzamos con el archivo de band_additional.

Primero representamos un histograma de algunas variables numéricas para visualizar cómo se distribuyen los valores de cada variable. Hacemos uns lista num_cols donde se recogen los datos de estas columnas, y luego con un bucle for recorremos las variables para representar los histogramas uno a uno. Con sns.histpot() creamos el histograma, haciendo kde=Tru cremos la curva de densidad y creamos un ancho de 30 bins.
Las conclusiones que podemos sacar son:
- La distribución de edad es tiene un pico entre los 30 y 40, y prácticamente disminuye a partri de los 60 y antes de los 20. La mayoría de clientes se encuentran en edad adulta activa.
- La mayoría de la duración de las llamadas son alrededor de 30-60segundos.
- La tasa de variación de empleo suele ser mayor de 1.
- La distribución de cons.price.idx suele tener valor 94. 

Para las varibles categóricas, lo que vamos a hacer es representar un histograma para mostrar las diferentes categórias que tiene cada una. En vez de un histograma creamos un gráfico de barras donde se muestra cuántas veces aparece cada categoría en cada variable. Con el argumento order=df_bank[col].value_counts().index organizmos las barrras según su frecuencia de mayor a menor. Con plt.xticks(rotation=45) rotamos las etiquetas de los ejes para que nose solapen.
Podemos extraer que:
- La mayor parte de los trabajos de los clientes son administrativo, siendo muy pocos los estudiantes.
- La mayoría de los clientes están casados.
- La mayoría de los clientes han estudiado una carrera universitaria.
- El contacto ha sido a través de telefóno móvil casi en el doble de clientes.
- Acerca de los resultados de campañas anteriores (poutcome) no se tiene apenas información (la mayoría es nonexistent). De los datos que se tienen, más fracaso que éxito 
- Los clientes que no se suscriben a productos son 7 veces más que los que sí lo hacen.

Una variable interesante de estudiar es cómo se comportar la variable y, que indica cuántos clientes se han suscrito y cuántos no. Para ello vamos a representar dicha variable con las categorías job, marital y education para analizar si algún grupo en concreto tiene mayor proporción de sí que de no. 

De nuevo creamos un gráfico por cada variable de tipo gráfico de barra, pero esta vez con dos variables  a representar , una la x que son las categorías que añadimos a la lista col y otra la 'y' que es la distribución de yes o no. De esta forma por cada categoría tenemos dos barras que indican los yes/no. También los ordenamos por frecuencia igual que en los gráficos anteriores.
Las conclusiones que obtenemos:
- Los estudiantes con los que ha contactado se han suscrito un porcentaje mayor que el resto de las categorías.
- En cuanto a estado civil, los solteros tienen una respuesta positiva mayor que el resto.
- Los clientes que tienen un curso profesional tienen una mayor respuesta positiva que el resto (aunque no notablemente mayor).



Vamos a hacer una matriz de correlación entre las variables. Esto nos da información sobre cómo se relacionan las distintas variables, es decir si sus valores cambian juntos o no. 
Para ello hacemos un mapa 2D donde obsevamos las variables y su índice de correlación, que va de -1 a 1. Usamos el código de colores para una mejor visualización (cuánto más rojo  índice más positivo y azul negativo). 

Para ello creamos un mapa de calor (heatmap) con seaborn, sns.heatmap(). Usamos la lista num_cols[] con las columnas numéricas uq ehemos definido anteriormente. Con annot=True mostramos los valores de correlación dentro de la matriz y con cmap='coolwarm' usamos la paleta de colores que va del azul al rojo.
La diagonal son 1 ya que cada variable consigo misma tiene una correlación de 1. 
El índice cercaco a 1 significa que cuando sube de valor una varible sube la otra, y por el contrario -1 significa que cuanto más sube uno, disminuye el otro.
Las conclusiones que podemos sacar son:
- Entre emp.var.rate y euribor3m: alta correlación positiva (0,82).Cuando sube la tasa de emplo, también sube el euribor3m (la tasa de interés).
- Entre emp.var.rate y cons.price.idx: alta correlación positiva (0,77). Mayor empleo implica precios más altos.
- Cons.price.ixd y euribor3m (0,57) correlación media-alta.
- Entre campaign y emp.var.rate (0,15)-ligera correlación positiva. Cuantas más campañas, se encuentran tasas de empleo algo mayores.
- Entre previous y emp.var.rate (-0,42)- correlación negativa media. Cuando la tasa de empleo es menor, más contactos previos se registran.
- Para age- con cualquier otra - el índice es cercano a 0 , esto quiere decir que la edad no tiene ninguna correlación el resto de variables.
Las conclusiones generales de la tabla pueden resumirse en que las variables e índices económicos (emp.var.rate, cos.price.idx y euribor3m) están muy relacionadas entre sí, ya que son indicadores financierons y económicos interdependientes. Por otro lado la variable previous está relacionada negativamente con estas 3 de antes, quiere decir que cuando la economía iba peor, se contactó más veces con clientes. Por su parte la edad no guarda ninguna relaciónn con las variables.



Ahora vamos a analizar el archivo customers.
Lo primero que hacemos es representar cada variable en un histograma para ver qué información nos puede dar:
- La variable income vemos que es bastante uniforme.
- Las variables de niño y adolescente en casa tienen los picos definidos en 0,1,2 , siendo los 3 del mismo tamaño, no nos aporta información.
- La distribución del número de visitas por mes también es bastante uniforme.

Para analizar la relacion de los clientes con la fecha tenemos que primero separar el valor de la fecha de las listas de datos. Para eso primero separamos el año con dt.year,  y reprsentamos el número de clientes que se unieron en los diferentes años. Vemos que en el 2012 hubo un mayor número de clientes que se dieron de alta.

Igual que con la hoja anterior, hacemos una matriz de correlación, pero en este caso no reaulta tan interesante porque vemos que las variables apenas están relacionadas entre ellas.


Lo siguiente que vamos a hacer es unir ambos archivos para obtener información a partir de ambos. 

Unimos con la función merge a partir de la columna (variable) que tienen en común que se llama ID en el archivo customers y id_ en el archivo bank_details. Al hacer el how='inner' conservamos solo los clientes que estén en ambos archivos.
Comprobamos la forma que tienen y mostramos las 5 primeras filas de la nueva tabla.

Vamos a extraer algunos gráficos para analizar qué información podemos obtener.

En primer lugar vamos a ver si los ingresos de los clientes tienen relación con que se hayan suscrito o no. Para ello hacemos un boxplot, que lo que hace es representar los valores alrededor de la mediana. Con las barras de error representa los valores atípicos que puede haber. Vemos que el rango es bastante parecido, por tanto esta relación no nos aporta mucha información. 

También comparamos la distribución de visitas web con la variable y. De nuevo observamos que para ambos resultados (no/yes) es bastante parecido el número de vistas a la página por mes.

Lo siguiente que vamos a comparar es el número de hijos que hay en casa. Para ello vamos a crear una nueva variable que sea TotalKids, una suma de los Kidhome y Teenhome. Representamos en un gráfico de valores. De nuevo no vemos una relación de que haya más contratos o no dependiendo del número de hijos. 

Finalmente hacemos una matriz de correlación con todas las variables juntas. Vemos que excepto las variables económicas y previous, no hay apenas correlación entre las distintas variables.




Como conclusión general del trabajo obtenemos que no hay ninguna variable (edad, ingresos, número de veces que se contacta...) que tenga una clara relación con los clientes que han contratado los servicios, que sería la variable más interesante a la hora de sacar resultados acerca de este estudio sobre los clientes. Aunque sí se han visto otras relaciones, como las relativas a las financieras (euribor3m, emp.var.rate y consprice.idx, entre otras) de variables que sí influyen entre ellas.
