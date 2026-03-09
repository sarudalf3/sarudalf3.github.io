---
layout: post
title:  "Gráficos Waterfall en R con ggplot2"
categories: ['Visualización de datos','Waterfall', 'R']
author: rmirfa
image: assets/images/posts/waterfall.jpg
---

En la industria minera y de procesos, el *tratamiento* de mineral es una métrica crítica. Para entender por qué no se alcanzó una meta o cómo se superó, se utilizan gráficos *Waterfall*.
Estos gráficos muestran cómo una línea de base inicial se ve afectada secuencialmente por factores positivos (ganancias de rendimiento) y negativos (fallas, paradas, stockpiles bajos).

Las aplicaciones Power BI y Tableau tienen gráficos para esto, como Data Scientists a menudo necesitamos la reproducibilidad y flexibilidad que ofrece R. En este post, replicaremos un gráfico waterfall del rendimiento de una planta de concentrado de cobre utilizando ggplot2.

El Desafío: Replicar el gráfico de rendimiento de una planta de concentrado en R
<div class="image_center mb-4 mt-2">
    <img src="/assets/images/posts/waterfall_01.png" alt="waterfall_01.jpg" style="max-width: 80%; max-height: 80%;"/> 
</div>

Figura 1: Gráfico waterfall del Tratamiento Planificado (Feb-25) vs. el Tratamiento Real, con todas las desviaciones intermedias.

Paso 1: Definir los Datos

Primero hay que *leer* los datos del gráfico original. En un flujo de trabajo real, estos datos vendrían de una base de datos de producción, en este ejemplo los codificaremos manualmente.

Nota:  Es necesario la columna de *Inicio* y *Fin* para cada barra para que el comando **geom_rect** funcione.

```R
library(tidyverse)
```

# 1. Definir los datos secuenciales según la imagen y convertir el factor ordenado (eje x / orden secuencial)

```R
datos_waterfall <- tribble(
  ~factor, ~valor, ~tipo,
  "Tratamiento Feb26 Plan", 1639, "Total inicial",
  "Rend. Molienda", 107, "Ganancia",
  "Equipo Programado", 383, "Ganancia",
  "Equipo No Programado", -117, "Pérdida",
  "Proceso Programado", -7, "Pérdida",
  "Proceso No Programado", -243, "Pérdida",
  "Bajo stockpile / Condición mineral", -103, "Pérdida",
  "Stand By", 0, "Neutro",
  "Tratamiento Real", 0, "Total final" #Calculo automático
) %>% mutate(factor = fct_inorder(factor))
```

# 2: Transformar datos
Calcular **Inicio** y **Fin** de cada barra. Esta es la parte clave. Para un gráfico waterfall, cada barra debe comenzar donde termina la anterior. **geom_col** no lo hace automáticamente; necesitamos **geom_rect**, y **geom_rect** necesita las coordenadas *ymin* y *ymax*.

Utilizaremos la función **accumulate()** de purrr (parte de tidyverse) para calcular estos valores de forma limpia.

# 3. Calcular las coordenadas de geom_rect (inicios y finales)

```R
waterfall_df <- datos_waterfall %>%
  mutate(
    fin = accumulate(valor, `+`),
    inicio = lag(fin, default = 0)
  )
```

# 4. Ajustar los totales iniciales y finales

```R
# En el gráfico original, las barras de 'Total' suben desde cero.
waterfall_df$inicio[1] <- 0
waterfall_df$inicio[nrow(waterfall_df)] <- 0

# El total final debe usar el valor final acumulado
waterfall_df$valor[nrow(waterfall_df)] <- waterfall_df$fin[nrow(waterfall_df)]
```

# 5. Generar el gráfico

Crear el Gráfico con **ggplot2**, usando **geom_rect**. También personalizaremos los colores para que coincidan (aproximadamente) con la paleta de la imagen original (Verdes para ganancia, Naranjas/Rojos para pérdida, Azules para totales).

```
ggplot(waterfall_df, aes(xmin = factor, xmax = factor, ymin = inicio, ymax = fin, fill = tipo)) +
  # Usamos geom_rect para las barras
  geom_rect(aes(xmin = as.integer(factor) - 0.4,
                xmax = as.integer(factor) + 0.4),
            color = "white", # Borde blanco
            size = 0.5) +
  # Añadir etiquetas de valor encima/debajo de las barras
  geom_text(aes(x = factor, y = fin,
                label = ifelse(valor == 0, 0, valor)), # Mostrar el cero de Standby
            vjust = ifelse(waterfall_df$valor >= 0, -1, 1.5), # Posición =/-
            size = 2.5, fontface = "bold") +
  # Personalización de colores (replicando la paleta de la imagen)
  scale_fill_manual(values = c(
    "Total inicial" = "#435970",  
    "Total final" = "#435970",    
    "Ganancia" = "#5D8579",      
    "Pérdida" = "#C15214",       
    "Neutro" = "lightgrey"
  )) +
  # Personalización de Ejes y Estilo
  scale_y_continuous(expand = expansion(mult = c(0, 0.1))) + # Añadir espacio arriba para etiquetas
  labs(
    title = "Waterfall tratamiento planta de Concentrado Feb-26",
    subtitle = "R/ggplot2",
    x = "",
    y = "kt") +
  scale_x_discrete(labels = function(x) str_wrap(x, width = 12)) + 
  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 0, hjust = 0.5, size = 8, lineheight = 0.8), # Rotar etiquetas de factor
    legend.position = "none", # Ocultar leyenda
    panel.grid.minor = element_blank(),
    plot.title = element_text(face = "bold")
  )
```

Resultados e interpretación
Al ejecutar el código, obtenemos el siguiente gráfico.

<div class="image_center mb-4 mt-2">
    <img src="/assets/images/posts/waterfall_01.png" alt="waterfall_02.jpg" style="max-width: 80%; max-height: 80%;"/> 
</div>

Principales Hallazgos (Insights):

Cumplimiento de Meta: Se logró un tratamiento real de 959 kt, superando el plan inicial de 939 kt en un 2.1%. A pesar de ser un resultado positivo, el análisis revela una alta variabilidad interna.

Eficiencia en Molienda: El factor Rend. Molienda (+107 kt) destaca una optimización en el proceso o una mejor calidad en las características del mineral alimentado.

Gestión de Disponibilidad: El éxito del periodo se apalancó en una excelente ejecución del Mantenimiento Programado (+283 kt), compensando con creces las desviaciones negativas.

Oportunidades de Mejora (Pérdidas): Se identificaron fugas de valor significativas en Procesos No Programados (-143 kt) y Fallas de Equipo (-117 kt). Estos eventos imprevistos suman una pérdida de 260 kt, señalando áreas críticas donde la implementación de modelos de Mantenimiento Predictivo podría capturar un valor latente considerable.
