---
layout: post
title:  "Tests para los parámetros en modelos de regresión usando Python"
categories: ['python', 'stats']
author: rmirfa
image: assets/images/posts/statsmodels.png
---
Una de las librerias más populares para estimar modelos de Machine learning en Python es scikit-learn; esta incluye una gran variedad de modelos, incluyendo modelos de regresión; aunque para los investigadores con formación estadística no hay disponible un output con tests individuales y grupales para los parámetros. 

No desesperes, hay alternativas. En este sentido, es preferible utiliar R-Gui, ya que puedes integrar ambas perspectivas, aunque como todo software su complejidad radica en aprender su lenguaje de programación (en buena forma no es muy diferente a Python, aunque su enfoque es estadístico -  las librerías para modelos de series de tiempo y GLM es un lujo). Una solución en Python es utilizar la librería statsmodels, y su enfoque es más cercano a lo que realiza R-Gui. 

Para mayor claridad exponemos un ejemplo. Sin ánimo de complejidad, se define un conjunto simple de datos . Para ambas librerías **X** e **y** es de la forma: 

```python
import numpy as np
from sklearn.linear_model import LinearRegression   #regresión en scikit-learn
import statsmodels.api as sm   #regresión en statsmodels

X = np.array([[1, 1], [1, 2], [2, 2], [2, 3]])

#Definir y = 3 +  x_0 + 2 * x_1 + error
y = 3 + np.dot(X, np.array([1, 2])) + np.random.rand(4)
```

La forma de estimación y los outputs disponibles en scikit-learn son los siguientes.

```python
reg = LinearRegression() #Define modelo de regresión lineal

reg.fit(X, y) #estima la ecuación de regresión para X e y

reg.score(X, y) #Calcula R2 (coef. determinación)
>> 0.9979
```

```python
reg.intercept_ #muestra param. de intercepto
>> 2.9796
```

```python
reg.coef_ #muestra param. asociados a var. independientes
>> array([0.9260, 2.1835])
```

Los outputs de scikit-learn son los parámetros del modelo (intercept_ y coef_), el coeficiente de determinación $$R^2$$.

En cambio, si queremos modelar usando statsmodels, tenemos que modelar de la siguiente manera. 

```python
X_stat = sm.add_constant(X, prepend=False)   #agrega constante al modelo
mod = sm.OLS(y, X_stat)   #aplica estimación por OLS (mínimos cuadrados)
res = mod.fit()   #ajusta modelo
print(res.summary())   #imprime resumen

>>
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                      y   R-squared:                       0.998
Model:                            OLS   Adj. R-squared:                  0.994
Method:                 Least Squares   F-statistic:                     239.1
Date:                Fri, 04 Mar 2022   Prob (F-statistic):             0.0457
Time:                        12:31:06   Log-Likelihood:                 4.0974
No. Observations:                   4   AIC:                            -2.195
Df Residuals:                       1   BIC:                            -4.036
Df Model:                           2                                         
Covariance Type:            nonrobust                                         
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
x1             0.9260      0.246      3.769      0.165      -2.196       4.048
x2             2.1835      0.174     12.567      0.051      -0.024       4.391
const          2.9796      0.288     10.341      0.061      -0.681       6.641
==============================================================================
Omnibus:                          nan   Durbin-Watson:                   2.000
Prob(Omnibus):                    nan   Jarque-Bera (JB):                0.667
Skew:                           0.000   Prob(JB):                        0.717
Kurtosis:                       1.000   Cond. No.                         10.4
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
```

Del output de **summary** se observa la estimación de los parámatros, el test conjunto F y los tests-t individuales. 

Podemos interpretar de los test-t individuales que los parámetros estimados para const, x1, x0 son significativamente distintos de 0 a un nivel de significación del 5% (p-valor &gt; 0.05); aunque para el test global F no se rechaza su hipótesis nula ($$\beta_{x1}=\beta_{x2}=0)$$ a un nivel de significación del 5%, (p-valor &lt; 0.05), poniendo en duda la validación del modelo (sólo la constante es significativamente distinto de 0 - ver interpretación del p-valor [aquí]({{site.baseurl}}/p-value)).

Esto es de utilidad para determinar si hay indicios que el modelo sea significativo, o que alguno de los parámetros no sea estadísticamente distinto de cero.

Además del resumen, tenemos estos output

```python
print("Parameters: ", res.params)
>> Parameters:  [0.9260 2.1835 2.9796]

print("R2: ", res.rsquared)
>> R2:  0.9979

print("Standard errors: ", res.bse)
>> Standard errors:  [0.2457 0.1738 0.2881]

print("Predicted values: ", res.predict())
>> Predicted values:  [ 6.0891  8.2726  9.1986 11.3821]
```

Inclusive se pueden realizar test para un subconjunto de parámetros:

```python
print("Test x1=x2=0 / F-test: ", print(res.f_test("x1 = x2 = 0"))
>> <F test: F=239.10, p=0.0457, df_denom=1, df_num=2>
```

Más información:

- [sci-kit learn doc](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
- [statsmodels doc](https://www.statsmodels.org/stable/regression.html)
- [Metrics and scores - scikit-learn](https://scikit-learn.org/stable/modules/model_evaluation.html)