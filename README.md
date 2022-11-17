## :computer:ICV TIME SERIES PREDICTION UNSING MACHINE LEARNING:green_book:  


## :scroll:Short results description

:green_book: The objective of this doccument is to predict the ICV timeseries using Machine Learning. To do this, 3 regression models were trained with data in *three-month frequency*, adittionally, the model performance were calculated using the **Mean Absolute Error**, which indicates **in average, how much the model predictions differ from real ones**

:green_book: Following, is shown the table with results.


**Table 1**

![](figuras_y_tablas/Tabla_1.PNG)

:green_book: From the previous table, can be concluded that the model with best performance is **RandomForest()**, because it has the least error value. 

## :minidisc: Data Exploration :minidisc:

#### :scroll:Resampling and icv timeseries reading

Initially, can be seen that the ICV timeseries is not in the same frequency as the predictor variables (**located in ‘variables_macro_trimestral.xlsx’**), because the ICV is in monthly and predictors are in quarterly frequencies. 

In base of the previous description, it’s necessary to answer **which of the two databases modify?** because the most convenient is that both timeseries have the same time frequency :hourglass:. In this case ICV timeseries will be converted from monthly to quarterly frequency. 

![](figuras_y_tablas/figura_1.PNG)

**Figura 1** se carga la información sobre el icv en **indice_df** y luego se realiza el resampling o cambio de frecuencia temporal a trimestral, y se guarda en el dataframe **quarterly_resampled_indice_df**

**NOTA:exclamation::** inicialmente, se entrenarán los modelos con datos trimestrales, ya que así se tendrán más datos y esto mejora la robustez del modelo; luego, más adelante en este documento, se mostrará cómo *se calcula el valor proyectado del icv del sistema financiero a un año* 

#### :book: Lectura y ajuste de variables 

:green_book: A continuación se carga la base de datos de variables y se eliminan las 3 primeras filas y la última, esto con el fin de que puedan ser agrupadas correctamente con los datos del icv y formar un sólo dataframe y así explorar más fácil los datos. 
**El objetivo es predecir cual será el icv promedio de los próximos 3 meses usando predictores o variables de la fecha actual.**

![](figuras_y_tablas/Figura_2.PNG)

**Figura 2**

:green_book: Luego, se deben unir los datos del ICV con las variables en un solo dataframe, además de agregar una columna numérica incremental para posteriores análisis, también se usa el comando **info()** para conocer los tipos de datos de las columnas y presencia de valores nulos

![](figuras_y_tablas/Figura_3.PNG)

**Figura 3**

:green_book: De la figura 3 se puede inferir que hay 4 columnas con valores nulos que deben ser modificados posteriormente mediante imputación. 

#### :bar_chart:Gráfico de correlaciones entre variables:chart_with_upwards_trend:

:green_book: Usando el comando **sns.pairplot()** se genera el siguiente gráfico, el cual muestra en gráficos de disepersión, la correlación existente entre todas las variables, inclusive el icv. 

![](figuras_y_tablas/Figura_4.png)

**Figura 4** 

:green_book: De la figura 4 se observa que la variable **icv_cartera_total** tiene una correlación positiva con la variable **Desempleo**, :bangbang:**pero su correlación con las demás variables no es fuerte**:bangbang:, hecho que se evidencia más adelante en este proyecto cuando se analicen las **relaciones y coeficientes del modelo de regresión lineal**, el cual es uno de los varios modelos posteriormente entrenados. 


## :memo:Imputación usando Regresión lineal:fountain_pen:

:green_book: En la Figura 4 también se puede observar que las variables **PIB** e **IPC** tienen una alta correlación positiva con el tiempo (a medida que pasan los años, el valor del PIB e IPC aumenta); por esta fuerte correlación, se ha tomado la decisión de usar un modelo de Regresión Lineal para reemplazar los datos faltantes en estas dos columnas; para esto, se desarrolló la función **regression_imputer()** la cual hace el proceso de imputación automático. 

:green_book: El código de la función se muestra a continuación, en dónda básicamente se entrena el modelo con los valores de la columna que no tengan valores nulos y luego de entrenado el modelo, éste se usa para predecir y sustituir los valores nulos por las predicciones realizadas por el modelo. 

![](figuras_y_tablas/Figura_5.png)

**Figura 5**

:green_book: Y a continuación se enseña cómo, usando la función **regression_imputer()** y un ciclo for, se reemplazan los valores nulos de las columnas **IPC** y **PIB**.

![](figuras_y_tablas/Figura_6.png)

**Figura 6** 

## :pencil2:Dividir datos entre 'entrenamiento' y 'testeo':scroll:

:arrow_right:Esta etapa es crucial en el desarrollo de cualquier modelo predictivo de Machine Learning, ya que **NUNCA** debe ser usado el 100% de los datos disponibles, sino realizar una partición y utilizar una parte en entrenar el modelo y la otra en testearlo para evaluar éste cómo se comporta haciendo predicciones con datos desconocidos.

**NOTA:exclamation::** **esta etapa es fundamental para garantizar la robustez estadística de cualquier modelo**

**NOTA 2:exclamation::** **Existen métodos más sofisticados de entrenamiento de modelos (CROSS VALIDATION) y optimización de hiperparametros:exclamation: (GRIDSEARCHCV), pero debido a la poca cantidad de datos disponibles para entrenar los modelos en este proyecto, se ha optado por una única partición de datos 'entrenamiento' - 'testeo'**

:arrow_right:A continuación, se procede a guardar en **x** la matriz de predictores de nuestros modelos y en **y** el vector de etiquetas o **'labels'**, en este caso el icv, y a partir de estas dos bases de datos, creamos las particiones de **'entrenamiento'** y **'testeo'** usando la función de sklearn **train_test_split()**.

![](figuras_y_tablas/Figura_7.png)

**Figura 7** 

:arrow_right:Como último paso antes de entrenar modelos, se deben imputar las columnas 'Exportaciones' e 'Importaciones'; en este caso usaremos los valores del promedio de la columna y la función **SimpleImputer()** de **sklearn**, ver siguiente figura.

![](figuras_y_tablas/Figura_8.png)

**Figura 8**


## :brain:Entrenamiento y evaluación de modelos:robot:

:arrow_right:Se decidió, en esta fase, entrenar y evaluar el desempeño de tres modelos de regresión, los cuales son:
    :bulb:RandomForestRegressor()
    :bulb:LinearRegression()
    :bulb:DecisionTreeRegressor()

:arrow_right:El desempeño de cada modelo se calculó usando el **Error Medio Absoluto**, el cual indica **En promedio, las predicciones del modelo difieren 'tantas' veces del valor real**. A continuación se muestra un ciclo for, el cual itera sobre los 3 modelos y genera el error que cada uno produce evaluado en los datos de 'testeo'

![](figuras_y_tablas/Figura_9.png)

**Figura 9**

## :notebook:INTERPRETACIÓN COEFICIENTES DEL MODELO DE REGRESIÓN LINEAL:mag_right:

:arrow_right:Pese a que el modelo de regresión lineal **no fue el modelo que mejor capacidad predictiva tuvo**, éste sin duda, es el modelo que mayor información nos entrega acerca del problema que queremos solucionar, puesto que **el valor de los coeficientes indica el grado de importancia que tiene cada variable a la hora de predecir la variable objetivo**, en este caso el icv. A continuación se muestra el desarrollo en código del modelo de regresión lineal, junto con los valores de los coeficientes de cada variable. 

![](figuras_y_tablas/Figura_Regresion_lineal.PNG)

**Figura regresión lineal**

:arrow_right:De la figura anterior, se puede inferir que la variable que más influencia tiene a la hora de predecir el icv es **Desempleo**; inclusive, desde un punto de vista simplificado, se podría decir que **es la única variable que tiene un impacto considerable a la hora de predecir el icv**, ya que su coeficiente es **varios ordenes de magnitud** mayor que los coeficientes de las demás variables. 


## Valor proyectado del icv a un año :chart_with_downwards_trend:

**NOTA:exclamation::** El código para la predicción del icv se encuentra en el archivo **icv_anual.ipynb** 

:arrow_right:Para realizar la predicción del icv a uno año, se debe realizar un 'resampling' similar al realizado previamente, pero esta vez no trimestral sino anual; adicionalmente este cambio de frecuencia temporal se debe realizar sobre las dos series de tiempo 'icv' y 'variables'. 

:arrow_right:El procedimiento de predicciones anuales no contiene nada diferente al procedimiento de predicciones trimestrales, la decisión de haber mostrado el procedimiento de desarrollo de modelos para frecuencias trimestrales es porque **con frecuencias trimestrales disponemos de más datos y por ello los modelos alcanzan mejores desempeños**. 

:arrow_right:Para la predicción anual se eligió el modelo **RandomForestRegressor()** porque éste fue el que mejor desempeño tuvo en las predicciones trimestrales; el error encontrado en las predicciones anuales fue **0.022005**, valor mayor al error trimestral, **puesto que se disponen de menos datos para entrenar el modelo**. La construcción del modelo se enseña a continuación. 

![](figuras_y_tablas/Figura_10.png)

**Figura 10**

:green_book:Para realizar la prediccion del icv anual del 2021, se utilizó el siguiente fragmento de código, y el resultado de esta predicción fue de **0.04638 = 4.638%**

![](figuras_y_tablas/Figura_11.png)

**Figura 11**

:green_book:Contrastando el valor predicho del icv con los valores reales, se encuentra poca diferencia, hecho que aumenta la credibilidad de los modelos acá entrenados; el artículo de **la república**, titulado **Estos son los bancos con mayores y menores índices de cartera vencida a julio de 2021** indica que para Julio del 2021 se tenía para Bancolombia un icv de 4,6%, valor similar al predicho por el modelo; pese a que sólo se tiene el valor de los primero 6 meses del icv para el 2021, es un estimativo significativo sobre la efectividad del modelo acá desarrollado. 

**NOTA:exclamation:: la fuente del artículo de la república, se encuentra en el archivo de texto plano adjunto al proyecto**


### :books:Variables adicionales en la predicción del icv:label:

:green_book:A la hora de considerar otras variables con capacidad predictiva para el icv, habrá que tener en cuenta que el icv es una **serie de tiempo** y por ende, se puede aprovechar la **teoría de modelación de series de tiempo** **(:bangbang: ARIMA & SARIMA :bangbang:)**, la cual emplea fuertemente los valores pasados o históricos de la serie de tiempo para predecir los futuros valores. por ejemplo, unas de las herramientas más usadas en series de tiempo son **medias móviles simples** y también **medias móviles exponenciales**.

:green_book:Por lo anterior, yo recomendaría inferir nuevas variables a partir de los valores pasados del icv, variables como: 

:herb: media móvil simple

:herb: media móvil exponencial

:herb: indice anterior

:herb: desviación estandard de la serie de tiempo 'n' periodos atrás

:maple_leaf:**Se pueden inferir ilimitados posibles predictores en base a los mismos datos históricos**, un ejemplo de uso extensivo de la teoría de predicción de series de tiempo son los modelos de trading financiero, los cuales analizan series de precios y deciden en base a ello tomar alguna decisión en el mercado. 









