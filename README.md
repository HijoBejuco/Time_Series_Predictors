# PROYECTO PARA PREDICCIÓN DEL ÍNDICE DE CARTERA VENCIDA USANDO MACHINE LEARNING Y TÉCNICAS DE TRANSOFORMACIÓN Y LIMPIEZA DE DATOS. 

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






















# COMMENT LATER THAT THE TECHNIQUE WE USED TO CHOOSE THE BEST MODEL IS SELECT THE BEST PREDICTING QUARTERS, BECAUSE WITH QUARTERS WE HAVE MORE DATA, AND THE MORE DATA WE HAVE, THE MORE STATISTICAL SIGNIFICANCE WE HAVE. 