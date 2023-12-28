---
layout: post
title:  "Como instalar jekyll para construir tu website en GitHub"
categories: ['github','jekyll','ubuntu']
author: rmirfa
image: assets/images/posts/jekyll.jpg
---
Si quieres construir tu propio website y no sabes como empezar, este post te puede ser de ayuda.

_Nota: Es necesario que conozcas conceptos de Linux, GitHub y git_. 
No te asustes, es más simple de lo que crees.

En estos tiempos de competitividad laboral, es necesario diferenciarnos de nuestros pares. Una buena alternativa es construir una website con tus proyectos realizados, usando como host la plataforma GitHub.

Antes de todo, ¿Que es Github?

GitHub es una plataforma de desarrollo colaborativo para alojar proyectos utilizando el sistema de control de versiones Git. Se utiliza principalmente para la creación de código fuente de programas informáticos. Ha tenido bastante aceptación por desarrolladores, y en el año 2018, Microsoft adquirió el sitio.

Si no tienes una cuenta, create una en el sitio de [GitHub](https://github.com/), es gratis.

El motivo de este post es instalar jekyll para la creación de websites, usando como host tu cuenta Github.
Puedes revisar temas de websites [aquí](https://jekyllrb.com/docs/themes/) y descargar la que más te guste (puedes clonar el repositorio desde github usando *git clone*).

¡Empecemos!

1. Si estás en el ambiente de windows, es necesario que instales Ubuntu. Para ello primero debes asegurarte que tu equipo tenga las caracteristicas necesarias para instalar Ubuntu.

    1.1 - Ir a Settings > System > About

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/01.jpg" alt="system device" style="max-width: 100%; max-height: 100%; height: 420px;" />
    </div>

    Debes tener un sistema de 64 bits y una OS mayor a 14393 (ver las actualizaciones de Windows).

    1.2 - Ir a Settings > Update & Security > For developers

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/02.jpg" alt="dev mode" style="max-width: 100%; max-height: 100%; height: 370px;" />
    </div>

    Aqui debes activar el modo de desarrollador.

    1.3 - Ir a activar o desactivar funciones de windows, este opción se activa buscando en el menú de windows

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/03.jpg" alt="turn features on" style="max-width: 100%; max-height: 100%; height: 400px;" />
    </div>

    Este opción abre una ventana emergente en la que tienes que seleccionar la opción Subsistema de Windows para Linux

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/04.jpg" alt="Linux subsystem" style="max-width: 100%; max-height: 100%; height: 240px;" />
    </div>

    Después de activar esta función, debes **reiniciar** el equipo. Ya estás en condiciones de instalar Ubuntu.


2. Dirigete a Microsoft Store y busca la versión de Ubuntu

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/05.jpg" alt="install ubuntu" style="max-width: 100%; max-height: 100%; width: 500px;" />
    </div>

3. Después de instalar Ubuntu, tienes que abrir la aplicación. Allí se te pedira un nombre de usuario y una contraseña (que usarás después).

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/06.jpg" alt="Ubuntu running" style="max-width: 100%; max-height: 100%; width: auto;" />
    </div>

    Ya estamos listos con la instalación de ububtu. Ahora tenemos que instalar jekyll en el ambiente de ruby para precargar diseños de paginas web.

4. Abrir una ventana de command prompt (tecla Windows + cmd + enter)

    <div class="image_center mb-4 mt-2">
        <img src="/assets/images/posts/jekyll/07.jpg" alt="cmd " style="max-width: 100%; max-height: 100%; width:220;" />
    </div>

    4.1 Ingresar a método de subsistema de Linux en Windows. Escribir en el command prompt
    
    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> > bash</p>
        </div>
    </div>

    4.2 Instalar y actualizar el repositorio brightbox. Cuando pregunte si quieres confirmar en agregar el reposotorio, presiona ENTER para ejecutar la instalación.
    
    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ sudo apt-add-repository ppa:brightbox/ruby-ng</p>
            <p> $ sudo apt-get update</p>
        </div>
    </div>

    *Nota: Si es tu primera vez, al usar sudo te pedira la contraseña de Linux ingresada en el paso 3*

    4.3 Instalar ruby versión 2.7

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ sudo apt-get install ruby2.7 ruby2.7-dev</p>
        </div>
    </div>

    4.4 Instalar gcc, g++ y make

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ sudo apt install build-essential</p>
        </div>
    </div>

    Para chequear si se instalaron bien los repositorios se utiliza

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ gcc --version</p>
            <p> $ g++ --version</p>
            <p> $ make --version</p>
        </div>
    </div>

    4.5 instalar jekyll y bundler

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ sudo gem install jekyll</p>
            <p> $ sudo gem install bundler</p>
        </div>
    </div>

    Para chequear si se instalaron bien los repositorios se utiliza

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ gem --version</p>
            <p> $ jekyll --version</p>
            <p> $ bundler --version</p>
        </div>
    </div>

    Y ya tienes todo instalado para empezar a utilizar jekyll.

5. Ir a la carpeta donde tienes un tema descargado ó donde vas a crear un tema básico. Para ello debes conocer estos comandos: 

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> #Ir a un directorio</p>
            <p> $ cd &lt;folder&gt; </p>
            <p> #Ir a la carpeta anterior</p>
            <p> $ cd ..</p>
            <p> #crear una nueva carpeta</p>
            <p> $ mkdir &lt;folder&gt; </p>
        </div>
    </div>

6. Instalar un tema en el directorio seleccionado. Aqui tienes 2 opciones.

    6.1 Crear un tema básico
    
    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> # Crear un nuevo sitio jekyll en ./folder </p>
            <p> $ jekyll new &lt;folder&gt; </p>
        </div>
    </div>


    6.2 Utilizar un tema ya descargado

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> # Utilizar tema instalado en la carpeta seleccionada</p>
            <p> $ bundle install</p>
        </div>
    </div>


7. Ejecutar tu website en un servidor local

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <p> $ bundle exec jekyll serve</p>
        </div>
    </div>

*Puedes leer la documentación de jekyll [aquí](https://jekyllrb.com/docs/).*