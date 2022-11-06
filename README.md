# PREDICCIÓN DEL ICV USANDO MACHINE LEARNING  

## Exploración de datos

#### Resampling y lectural del icv

Inicialmente observamos que la serie de tiempo del índice de cartera vencida (icv) no está en la misma frecuencia temporal que las variables suministradas, puesto que el icv está mensual y las variables están trimestrales.

Por lo anterior, nos vemos obligados a tomar la primer decisión importante en este proyecto: **¿cual de las dos bases de datos modificar?** ya que lo más conveniente es que ambas series de tiempo tengan la misma frecuencia temporal. Para el caso de las variables, al ser datos macroeconómicos trimestrales, es más dificil convertirlas a mensuales, por lo tanto se decide realizar un **resampling** al icv, para convertirlo de mensual a trimestral. 

![](figuras_y_tablas/figura_1.PNG)

**Figura 1** se carga la información sobre el icv en **indice_df** y luego se realiza el resampling o cambio de frecuencia temporal a trimestral, y se guarda en el dataframe **quarterly_resampled_indice_df**

**NOTA:** inicialmente, se entrenarán los modelos con datos trimestrales, ya que así se tendrán más datos y esto mejora la robustez del modelo; luego, más adelante en este documento, se mostrará cómo *se calcula el valor proyectado del icv del sistema financiero a un año* 

#### Lectura y ajuste de variables

A continuación se carga la base de datos de variables y se eliminan las 3 primeras filas y la última, esto con el fin de que puedan ser agrupadas correctamente con los datos del icv y formar un sólo dataframe y así explorar más fácil los datos. 
**El objetivo es predecir cual será el icv promedio de los próximos 3 meses usando predictores o variables de la fecha actual.**

![](figuras_y_tablas/Figura_2.PNG)

**Figura 2**

Luego, debemos unir los datos del ICV con las variables en un solo dataframe, además de agregar una columna numérica incremental para posteriores análisis y también usamos el comando info() para conocer los tipos de datos de las columnas y presencia de valores nulos

![](figuras_y_tablas/Figura_3.PNG)

**Figura 3**

De la figura 3 se puede inferir que hay 4 columnas con valores nulos que deben ser modificados posteriormente mediante imputación. 

#### Gráfico de correlaciones entre variables 

Usando el comando **sns.pairplot()** se genera el siguiente gráfico, el cual muestra en gráficos de disepersión, la correlación que existe entre todas las variables, inclusive el icv. 

![](figuras_y_tablas/Figura_4.png)

**Figura 4** 

De la figura 4 se observa que la variable **icv_cartera_total** tiene una correlación positiva con la variable **Desempleo**, pero su correlación con las demás variables no es fuerte, hecho que se evidencia más adelante en este proyecto cuando se analicen las **relaciones y coeficientes del modelo de regresión lineal**, el cual es uno de los varios modelos posteriormente entrenados. 



## Imputación usando Regresión lineal 

En la Figura 4 también se puede observar que las variables **PIB** e **IPC** tienen una alta correlación positiva con el tiempo (a medida que pasan los años, el valor del PIB e IPC aumenta); por esta fuerte correlación, se ha tomado la decisión de usar un modelo de Regresión Lineal para reemplazar los datos faltantes en estas dos columnas; para esto, se desarrolló la función **regression_imputer()** la cual hace el proceso de imputación automático. 

El código de la función se muestra a continuación, en dónda básicamente se entrena el modelo con los valores de la columna que no tengan valores nulos y luego de entrenado el modelo, éste se usa para predecir y sustituir los valores nulos por las predicciones realizadas por el modelo. 

![](figuras_y_tablas/Figura_5.png)






















# COMMENT LATER THAT THE TECHNIQUE WE USED TO CHOOSE THE BEST MODEL IS SELECT THE BEST PREDICTING QUARTERS, BECAUSE WITH QUARTERS WE HAVE MORE DATA, AND THE MORE DATA WE HAVE, THE MORE STATISTICAL SIGNIFICANCE WE HAVE. 