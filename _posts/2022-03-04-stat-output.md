---
layout: post
title:  "Tests de los parámeteos en modelos de regresión en Python"
categories: ['python', 'stats']
author: rmirfa
image: assets/images/posts/statsmodels/statsmodels.png
---
Una de las librerias más populares para estimar modelos de Machine learning en Python es scikit-learn; esta incluye una gran variedad de modelos, incluyendo modelos de regresión; aunque para investigadores con formación estadísticos se extraña el output de los tests individuales y grupales de los parámetros. 

La forma de estimación y los outputs disponibles en scikit-learn son los siguientes:

```python
import numpy as np
from sklearn.linear_model import LinearRegression

X = np.array([[1, 1], [1, 2], [2, 2], [2, 3]])

#Definir y = 1 * x_0 + 2 * x_1 + 3
y = np.dot(X, np.array([1, 2])) + 3

reg = LinearRegression() #Define modelo de regresión lineal
reg.fit(X, y) #estima la ecuación de regresión para X e y

reg.score(X, y) #Calcula R2 (coef. determinación)
1.0

reg.coef_ #muestra param. asociados a var. independientes
array([1., 2.])

reg.intercept_ #muestra param. de intercepto
3.0

reg.predict(np.array([[3, 5]]))#Predecir para X=[[3, 5]]
array([16.])
```

En el modulo de sci-kit learn podemos observar los parámetros estimados en la regresión, aunque para construir los tests de hipótesis asociados a los parámetros de regresión no están disponibles. Si queremos obtener dicha información debemos hacer la estimación del modelo de regresión utilizando la libreria statsmodels. 



Las librerías para modelación 
Al igual que R, hay variadas Las librerias

Python posee muchas librerías, auque para 

- [sci-kit learn doc](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
- [statsmodels doc](https://www.statsmodels.org/stable/regression.html)