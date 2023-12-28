---
layout: post
title:  "Suavizamiento exponencial: Tipos y como usarlo en Python"
categories: ['time series', 'stats']
author: rmirfa
image: assets/images/posts/smoothing.png
---

Uno de los objetivos de la estadística es reducir la incertidumbre mediante técnicas de modelación y predicción; estas dependen de la caracteristica del dato a tratar. Gran parte de la estadística clásica supone que los registros capturados se consideran independientes y de igual distribución (iid); pero ¿qué pasa cuando estos supuestos no se cumplen?

Hay una clase de datos que se llaman series de tiempo, y no es más que una medición de interés en distintos momentos en el tiempo; estos deben ser equidistantes entre sí (una secuencia diaria, mensual, etc..), donde se infiere que la observación en el instante $t$ depende de las mediciones anteriores $t-1$, $t-2$,...; siendo una violación del supuesto de independencia entre las observaciones.

## Modelos ingenuos de tiempo

Dentro de este campo de modelación, se consideran *modelos ingenuos* a aquellos que pueden ser expresadas por tres componentes: Tendencia (T), estacionalidad (S) y un residuo (R) llamado error aleatorio.

Es posible tener los siguientes casos:

1. $$X_{t} = T_{t} + S_{t} + R_{t}, \qquad \text{efecto aditivo}$$
2. $$X_{t} = T_{t} * S_{t} * R_{t}, \qquad \text{efecto multiplicativo}$$
3. $$X_{t} = T_{t} + S_{t} * R_{t}, \qquad \text{efecto mixto}$$

Se asume que $R_{t}$ es una componente aleatoria con media $0$ y varianza constante $c$.

Unos de los modelos utilizados es de suavizamiento exponencial.

Este tipo de modelos es de procedimientos automáticos de predicción, y son ampliamente usados por su simpleza y aplicabilidad en diversas áreas.

Se describen los suavizamientos exponencial simple, doble y Holt-Winters.

1. Suavizamieto exponencial simple

    Este tipo de suavizamiento es útil cuando es relativamente constante, donde se asume que tiene una curva del tipo $X_{t}= \alpha + \epsilon_t,$ donde $\alpha$ es un parámetro de media que oscila ocasionalmente en el tiempo y $\epsilon_t$ un error aleatorio.

2. Suavizamiento exponencial doble

    Este tipo de serie de tiempo considera una componente de tendencia, esto es $X_{t}=\alpha + T_t + \epsilon_t,$ donde $T_t$ es la componente de tendencia y depende del instante $t$. En estos casos se asume una tendencia lineal en la serie de datos.

3. Suavizamiento de Holt Winters

    A este nivel de suavizamiento es posible incorporar una componente ciclico -comunmente llamado estacional. Aquí la ecuación toma la forma $X_{t}=\alpha + T_t + S_{t-s} + \epsilon_t$, donde $T_t$ es la componente de tendencia que depende del instante $t$, y de un periodo $s$ asumiendo ese instante de periodicidad. El caso anterior se denomina efecto aditivo, donde la interacción entre las componentes se da de manera lineal, aunque existe la versión multiplicativa, y consiste en la expresión $X_{t}=(\alpha * T_t)+ S_{t-s} * \epsilon_t$.

Para información de como implimentar este tipo de modelación en Python revisar el siguiente [github repo](https://github.com/sarudalf3/ExponencialSmoothing).

References:

- [Forecasting: Principles and Practice](https://otexts.com/fpp2/expsmooth.html)
- [stats model library](https://www.statsmodels.org/dev/examples/notebooks/generated/exponential_smoothing.html)
- [Apuntes personales](../assets/cv/smoothing.pdf)