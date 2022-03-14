---
layout: post
title:  "Como crear ambientes virtuales en Python usando powershell"
categories: ['python','pip']
author: rmirfa
image: assets/images/posts/venv/venv.png
---
Un entorno virtual es un espacio donde podemos instalar librerías específicas para diferentes proyectos, es decir, puedes tener instaladas librerías distintas de la instalación base de Python. Uno de los motivos para los entornos virtuales es tener espacios de trabajo en las cuales puedes instalar distintas versiones de librerías para no crear errores entre ellas, y mantener separado los distintos ambientes de trabajo de los proyectos.

Para la creación de un entorno vitual es 

Para la creación de entornos virtuales se recomienda utilizar el módulo venv, el cual viene instalado por defecto con la librería estándar de Python desde la versión 3.3. Por otro lado, la instalación de paquetes se realiza mediante la herramienta pip, la cual viene incluida por defecto a partir de Python 3.4. Como los comandos que vamos a ver en este post utilizan estas dos herramientas, se asume que se trabaja como mínimo con la versión 3.4.