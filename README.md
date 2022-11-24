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

**Figure 1** Information about the ICV is saved into **indice_df** and then the resampling to quarterly is done, after is saved into the dataframe **quarterly_resampled_indice_df**

**NOTE:exclamation::** Initially, we will train the models with quarterly data, then, we will show how to project/predict the icv data yearly. 

#### :book: Reading variables (Predictors)

:green_book: Following we read the predictors, and first we drop the first 3 rows and the last one so they match with the labels (icv)

![](figuras_y_tablas/Figura_2.PNG)

**Figure 2**

:green_book: Then we concat the predictors and labels into a single dataframe, and we add an incremental numerical column so it represents the numerical period.

![](figuras_y_tablas/Figura_3.PNG)

**Figure 3**

:green_book: From figure 3 we infer that there are 4 columns with null values which must be imputed

#### :bar_chart:Correlation between variables:chart_with_upwards_trend:

:green_book: We generated the following plot so we see the correlations between variables, icv included 

![](figuras_y_tablas/Figura_4.png)

**Figura 4** 

:green_book: We see **icv** has a positive correlation with **Desempleo** variable, :bangbang: **but the correlation of icv with the remaining variables isn’t so strong** :bangbang:


## :memo:Imputation using Linear Regression:fountain_pen:

:green_book: Also from figure 4 we see variables**PIB** and **IPC** have a strong correlation with time (the more time the higher the values), for this reason we chose to use Linear Regression to impute the null values, to do this, we developed the function **regression_imputer()** which makes the imputation automatically.


![](figuras_y_tablas/Figura_5.png)

**Figure 5**

:green_book: Figure 6 shows how to use the **regression_imputer()** function

![](figuras_y_tablas/Figura_6.png)

**Figure 6** 

## :pencil2:Training 'entrenamiento' & Test 'testeo' Data Split:scroll:


:arrow_right:Following we save in **x** the predictors matrix and in **y** the labels vector (icv), and from these two data arranges, we create the splits **’entrenamiento’** for model training and **testeo** for model testing, using sklearn function **train_test_split()**.

![](figuras_y_tablas/Figura_7.png)

**Figure 7** 

:arrow_right:To impute the remaining columns with null values, we used the **SimpleImputer()** function

![](figuras_y_tablas/Figura_8.png)

**Figura 8**


## :brain:Training and Model Evaluation:robot:

:arrow_right:We decided to use three models:

:bulb:RandomForestRegressor()

:bulb:LinearRegression()

:bulb:DecisionTreeRegressor()

:arrow_right: We used mean absolute error to calculate the error of every model. Following we show a for loop which trains and tests the models, and then generates a table with the results. 

![](figuras_y_tablas/Figura_9.png)

**Figure 9**

## :notebook:LINEAR REGRESSION MODEL COEFFICIENTS INTERPRETATION:mag_right:

:arrow_right: Linear regression models provide important information and interpretations from the problem we are modelling, because the values of it’s coefficients indicate the weight each variable has in the final prediction of the model, so the greater the coefficient, the more important it is. Following we show the Linear Regressor construction and the values of its coefficients. 

![](figuras_y_tablas/Figura_Regresion_lineal.PNG)

**Linear Regression Figure**

:arrow_right:From the last figure we can conclude that the most important variable on icv predictions is **Desempleo** (Unemployment). 


## Predicting annual icv :chart_with_downwards_trend:

**NOTA:exclamation::** code for annual icv predictions is in **icv_anual.ipynb** file

:arrow_right: Predicting annual icv it’s the same as quarterly but transforming the original dataset into annual resampling. 


:arrow_right: To generate annual predictions we chose the **RandomForestRegressor()**, because it is the model with better performance in quarterly predictions

![](figuras_y_tablas/Figura_10.png)

**Figure 10**

:green_book: below we show the code to make the annual predictions, which result for the year 2021 is: **0.04638 = 4.638%**

![](figuras_y_tablas/Figura_11.png)

**Figure 11**












