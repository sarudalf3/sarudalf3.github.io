---
layout: post
title:  "Suavizamiento exponencial: Tipos y cómo usarlo en Python"
categories: ['time series', 'stats']
author: rmirfa
image: assets/images/posts/smoothing.png
---

El objetivo fundamental de la estadística es reducir la incertidumbre mediante técnicas de modelación y predicción, las cuales dependen intrínsecamente de la naturaleza de los datos. Mientras que la estadística clásica suele asumir que las observaciones son independientes e idénticamente distribuidas (iid), ¿qué sucede cuando estos supuestos no se cumplen?

Las **series de tiempo** son secuencias de mediciones de una variable de interés capturadas en intervalos constantes (diarios, mensuales, etc.). En este tipo de datos, la observación en el instante $t$ suele depender de los valores previos ($t-1, t-2, \dots$), lo que representa una violación directa al supuesto de independencia.

## Componentes de una Serie de Tiempo

En el análisis de series de tiempo, los modelos se descomponen tradicionalmente en tres elementos clave: **Tendencia ($T$)**, **Estacionalidad ($S$)** y un componente aleatorio o **Residuo ($R$)**.

Dependiendo de cómo interactúan estas componentes, podemos identificar tres estructuras principales:

1. **Efecto Aditivo:** $$X_{t} = T_{t} + S_{t} + R_{t}$$
2. **Efecto Multiplicativo:** $$X_{t} = T_{t} \times S_{t} \times R_{t}$$
3. **Efecto Mixto:** $$X_{t} = T_{t} + S_{t} \times R_{t}$$

Para estos modelos, se asume que $R_{t}$ es una componente aleatoria (ruido blanco) con media $0$ y varianza constante $\sigma^2$.

## Métodos de Suavizamiento Exponencial

El suavizamiento exponencial agrupa una serie de procedimientos automáticos de predicción, ampliamente valorados por su simplicidad y eficacia en diversas áreas. A continuación, se describen sus tres variantes principales:

### 1. Suavizamiento Exponencial Simple (SES)
Este método es útil cuando la serie es relativamente estable y no presenta una tendencia clara ni estacionalidad. Se asume que el proceso sigue la forma:
$$X_{t}= \ell_t + \epsilon_t$$
Donde $\ell_t$ representa el nivel medio que oscila ocasionalmente en el tiempo y $\epsilon_t$ es el error aleatorio.

### 2. Suavizamiento Exponencial Doble (Holt)
Este tipo de serie considera una componente de **tendencia lineal**. La estructura base se define como:
$$X_{t}= \ell_t + b_t + \epsilon_t$$
Donde $b_t$ representa la pendiente o tendencia en el instante $t$. Es ideal para datos que muestran un crecimiento o decrecimiento sostenido en el tiempo.

### 3. Suavizamiento de Holt-Winters
A este nivel de suavizamiento se incorpora una componente cíclica, comúnmente llamada **estacionalidad**. La ecuación toma la forma:
$$X_{t}= \ell_t + b_t + s_{t-m} + \epsilon_t$$
Donde $s_{t-m}$ representa el efecto estacional de un periodo $m$ atrás. El caso anterior describe un efecto aditivo (interacción lineal), aunque también existe la versión multiplicativa, expresada como:
$$X_{t} = (\ell_t + b_t) \times s_{t-m} + \epsilon_t$$

## Implementación en Python con Statsmodels

Para aplicar estos modelos, la librería más robusta es `statsmodels`. A continuación, se muestra un ejemplo básico de cómo cargar un modelo de **Holt-Winters** (Suavizamiento Exponencial Triple) para realizar predicciones:

```python
import pandas as pd
from statsmodels.tsa.holtwinters import ExponentialSmoothing

# Supongamos que 'df' es un DataFrame con un índice de tiempo y una columna 'ventas'
# 1. Definir el modelo
# Trend: 'add' (aditivo), Seasonal: 'mul' (multiplicativo),
# periods: frecuencia estacional
model = ExponentialSmoothing(df['ventas'], 
                             trend='add', 
                             seasonal='mul', 
                             seasonal_periods=12)

# 2. Ajustar el modelo a los datos
model_fit = model.fit()

# 3. Realizar una predicción para los próximos 12 meses
forecast = model_fit.forecast(12)

print(forecast)
```

En este [repo github](https://github.com/sarudalf3/ExponencialSmoothing) hay un ejemplo de estimación por suavizamiento exponencial.

**Bibliografía**

- [Forecasting: Principles and Practice](https://otexts.com/fpp2/expsmooth.html)
- [Statsmodels documentation](https://www.statsmodels.org/dev/examples/notebooks/generated/exponential_smoothing.html)
- [Apuntes personales](../assets/cv/smoothing.pdf)