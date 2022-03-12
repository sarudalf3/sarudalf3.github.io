---
layout: post
title:  "Como construir tu website en GitHub"
categories: ['github','jekyll','Ubuntu']
author: rmirfa
image: assets/images/posts/jekyll/github_page.jpg
---
Si quieres construir tu propio website y no sabes como, este post te puede ser de ayuda.

_Nota: Es necesario que conozcas conceptos de Linux, GitHub y git_. 
No te asustes, es más simple de lo que crees.

En estos tiempos de competitividad laboral, es necesario diferenciarnos de nuestros pares. Una buena alternativa es construir un dossier de proyectos, y uno de estos es la construcción tu propio website, usando como host la plataforma GitHub.

Antes de todo, ¿Que es Github?

GitHub es una plataforma de desarrollo colaborativo para alojar proyectos utilizando el sistema de control de versiones Git. Se utiliza principalmente para la creación de código fuente de programas informáticos. Ha tenido bastante aceptación por desarrolladores, y en el año 2018, Microsoft adquirió el sitio.

Si no tienes una cuenta, create una en el sitio de [GitHub](https://github.com/), es gratis.

¡Empecemos!

1. Si estás en el ambiente de windows, es necesario que instales Ubuntu. Para ello primero debes asegurarte que tu equipo tenga las caracteristicas necesarias para instalar Ubuntu.

    1.1 - Ir a Settings > System > About

    <div id="img01"> 
        <img src="/assets/images/jekyll/01.jpg" alt="Check1" style="max-width: 100%; max-height: 100%" />
    </div>
    
    Debes tener un sistema de 64 bits y una OS mayor a 14393 (ver las actualizaciones de Windows).

    1.2 - Ir a Settings > Update & Security > For developers
    <div id="img02"> 
        <img src="/assets/images/jekyll/02.jpg" alt="Check1" style="max-width: 100%; max-height: 100%" />
    </div>

    Aqui debes activar el modo de desarrollador.

    1.3 - Ir a activar o desactivar funciones de windows, este opción se activa buscando en el menú de windows

    <div id="img03"> 
        <img src="/assets/images/jekyll/03.jpg" alt="Check1" style="max-width: 100%; max-height: 100%" />
    </div>

    Este opción abre una ventana emergente en la que tienes que seleccionar la opción Subsistema de Windows para Linux

    <div id="img04"> 
        <img src="/assets/images/jekyll/04.jpg" alt="Check1" style="max-width: 100%; max-height: 100%" />
    </div>

    Después de activar esta función, debes reiniciar el equipo. Ya estás en condiciones de instalar Ubuntu.


2. Dirigete a Microsoft Store y busca la versión de Ubuntu

    <div id="img05"> 
        <img src="/assets/images/jekyll/05.jpg" alt="Check1" style="max-width: 100%; max-height: 100%" />
    </div>

3. Después de instalar Ubuntu, tienes que abrir la aplicación. Allí se te pedira un nombre de usuario y una contraseña (que usarás después).

    <div id="img06"> 
        <img src="/assets/images/jekyll/06.jpg" alt="Check1" style="max-width: 100%; max-height: 100%" />
    </div>

Ya estamos listos. Ahora tenemos que instalar jekyll, en ambiente de ruby para precargar diseños de paginas web