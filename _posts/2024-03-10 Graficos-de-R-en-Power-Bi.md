---
layout: post
title:  "Visualizaciones en Power Bi usando R"
categories: ['R','Power BI','DataViz']
author: rmirfa
image: assets/images/posts/pbix.jpg

---

# Cómo Crear Visualizaciones de R en Power BI

## Introducción

En este artículo, exploraremos cómo integrar el poder estadístico y gráfico de R con las capacidades interactivas de visualización de Power BI. R es un lenguaje y entorno para cálculos estadísticos y gráficos que cuando se combina con Power BI permite realizar análisis complejos y mayor personalización en las visualizaciones de datos.

## Instalación de R

Antes de comenzar, necesitarás tener R instalado en tu equipo. Puedes descargar e instalar R gratuitamente desde el repositorio de CRAN [(CRAN R website)](https://cran.r-project.org/). Asegúrate de tener la versión de R que sea compatible con Power BI Desktop.

## Habilitación de Objetos Visuales de R en Power BI Desktop

Una vez instalado R, Power BI Desktop lo habilitará automáticamente. Para verificar que R esté habilitado correctamente, sigue estos pasos:

1. En Power BI Desktop, selecciona Archivo > Opciones y configuración > Opciones.
2. En el lado izquierdo de la página Opciones, selecciona Script de R.
3. Comprueba que la instalación local de R esté especificada en Directorios detectados de inicio R.

La siguiente figura muestra un ejemplo de las opciones de configuración:

<div class="image_center mb-4 mt-2">
    <img src="/assets/images/posts/pbix/r_install.jpg" alt="pbix insert" style="max-width: 100%; max-height: 100%;"/> 
</div>

## Creación de Objetos Visuales de R
Para crear un objeto visual de R en Power BI Desktop:

1. Obten un conjunto de datos en Power BI. A modo de ejemplo simulé 20 observaciones de distribuciones normales con distinta media y desviación estándar de 5.

    <div class="image_center mb-4 mt-2">
    <img src="/assets/images/posts/pbix/database.jpg" alt="pbix db" style="max-width: 100%; max-height: 100%;"/> 
    </div>

2. Selecciona el icono Objeto visual de R en el panel Visualizaciones.

    En la ventana Habilitar objetos visuales de script que aparece, selecciona Habilitar.

    Agrega los campos desde el panel Valores que quieras consumir en el script de R. En el ejemplo se seleccionan x e y, con opción de no resumir el campo.
     
    <div class="image_center mb-4 mt-2">
    <img src="/assets/images/posts/pbix/Viz.jpg" alt="pbix viz" style="max-width: 100%; max-height: 100%;"/> 
    </div>

3. Ingresa el código de programación en la sección del script R del gráfico.

    Tomar en cuenta que el dataframe que incluye los campos seleccionados, eliminando los duplicados. Un ejemplo de visualización es como muestra la figura.

    <div class="image_center mb-4 mt-2">
    <img src="/assets/images/posts/pbix/plot.jpg" alt="pbix plot" style="max-width: 100%; max-height: 100%;"/> 
    </div>

4. Seguridad de Scripts R
    
    Es importante considerar la seguridad de los scripts R, ya que podrían contener código que presente riesgos para la seguridad o la privacidad. Estos riesgos existen principalmente en la fase de creación cuando el autor del script lo ejecuta en su propio equipo.

5. Publicación de reportes en Workspaces

    Tener en cuenta que en la versión Power BI no están disponibles todas las librerías de R. Para más detalle, leer la [Documentación de Power BI](https://learn.microsoft.com/en-us/power-bi/connect-data/service-r-packages-support) al respecto.  
    
    Adicionalmente, no es posible exportar las gráficas desarrolladas en Script R desde los workspaces a archivos con los formatos .pdf y .pptx


## Conclusión

Crear visualizaciones de R en Power BI puede enriquecer tus informes con análisis avanzados y visualizaciones interactivas. Con estos pasos, puedes comenzar a explorar la integración de R en tus proyectos de Power BI.