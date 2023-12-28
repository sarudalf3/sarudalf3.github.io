---
layout: post
title:  "Como crear ambientes virtuales en Python usando powershell"
categories: ['python','pip']
author: rmirfa
image: assets/images/posts/venv.png
---

Un entorno virtual es un espacio donde podemos instalar librerías específicas para diferentes proyectos en ambientes diferentes al ambiente base de Python. Uno de los motivos de usar entornos virtuales es tener espacios de trabajo en las cuales puedes instalar distintas versiones de librerías y así no generar conflictos de instalación entre ellas y mantener separado distintos ambientes de trabajo según tus poryectos.

Para la creación de un entorno vitual es posible mediante cmd, bash y powershell; este post se enfoca en el uso de este último.

Python es un lenguaje en el que se agregan librerias según tus necesidades, aunque para ello primero debemos saber que es pip.

PIP es el sistema de administración de librerías usado para instalar y administrar paquetes/librerias en el lenguaje de Python. Estos archivos son almacenados en un gran repositorio en línea, denominado Python Package Index (PyPI).  Para versiones superiores a 2.7.9 y 3.4 de Python esta librería viene instalado por defecto. 

Si no tienes instalado pip, te explicamos que hay dos alternativas de instalación:

1. **ensurepip**

    Python viene con el módulo ensurepip, quien puede instalar pip en el ambiente de Python . Abrir Powershell y escribir 

    <div class=" mt-2 mb-4">
        <pre>py -m ensurepip --upgrade </pre>
    </div>

    Más detalles de como ensurepip usar la librería, revisar la documentación de la librería. La opción <pre_inline>--upgrade</pre_inline> instala la versión más reciente de la librería.

2. **git-pip.py**

    2.1 Esto es un script de Python que usada para instalar pip. Primero debes descargar este [script](https://bootstrap.pypa.io/get-pip.py).

    2.2 Abrir un terminal/comando de Powershell y usar <pre_inline>cd</pre_inline> para ir al directorio donde esta el script de Python guardado en el paso anterior, usando el comando 
    
    <div class=" mt-2 mb-4">    
        <pre>cd "&lt;get-pip folder&gt;"</pre> 
    </div>

     El acrónimo de *change directory* es <pre_inline>cd</pre_inline>.
    
    2.3 Ejecutar 

    <div class=" mt-2 mb-4"> 
        <pre>py get-pip.py</pre>
    </div>    

Con la instalación de pip vamos a revisar las librerías instaladas.

Ahora explicaremos como utilizar un ambiente virtual usando Powershell.

1. Opciones de ejecución en Windows. Por defecto Windows tienen una política de ejecución restringida. Para cambiar su configuración incluir lo siguiente en la consola de Powershell

    <div class=" mt-2 mb-4"> 
        <pre>Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser</pre>
    </div>  

2. Chequear si la instalación base de Python tienes instalada la librería virtualenv

    <div class=" mt-2 mb-4"> 
        <pre>pip list</pre>
    </div>  

    Esto muestra todas las librerías instaladas. Si en la lista no aparece la librería virtualenv, hay que instalarla usando el código
    
    <div class=" mt-2 mb-4"> 
        <pre>pip install virtualenv</pre>
    </div>
    
3. Crear un ambiente virtual. Para ello tenemos que seleccionar un nombre para el ambiente virtual <pre_inline>venv_name</pre_inline> y el directorio donde lo queremos guardar <pre_inline>&lt;folder_to_save_venv&gt;</pre_inline>. 

    <div class=" mt-2 mb-4"> 
        <pre>python -m venv &lt;folder_to_save_venv&gt;\venv_name </pre>
    </div>

4. Activar el ambiente virtual. Tomando el directorio y nombre del ambiente del paso anterior, escribimos la sentencia 

    <div class=" mt-2 mb-4"> 
        <pre>&lt;venv_folder&gt;\venv_name\Scripts\activate.ps1 </pre>
    </div>

    Al momento de activar el ambiente virtual el nombre de este aparece al comienzo de cada sentencia (antes del texto PS y destacado en otro color).

    <div class="console mt-2 mb-4">
        <div class="consolebody">
            <span style="color:#00D100"> (venv_name) </span>PS &lt;active_folder&gt; >
        </div>
    </div>

    Dentro del ambiente virtual ahora podemos instalar librerias usando pip.

5. Desactivar un ambiente virtual. Solo debemos escribir el comando    

    <div class=" mt-2 mb-4"> 
        <pre>deactivate</pre>
    </div>

6. Activar un ambiente virtual ya creado

    <div class=" mt-2 mb-4"> 
        <pre>&lt;other_venv_folder&gt;\other_venv_name\Scripts\activate.ps1 </pre>
    </div>

7. Obtener las librerias instaladas en un ambiente virtual y guardarlos en el directorio &lt;folder_to_save_file&gt;\requirements.txt. Para esto hay que tener activo el entorno virtual en la terminal.

    <div class=" mt-2 mb-4"> 
        <pre>pip freeze &gt; &lt;folder_to_save_file&gt;\requirements.txt </pre>
    </div>

8. Instalar las librerías que se están listadas en el archivo &lt; &lt;folder_to_load_file&gt;\requirements.txt

    <div class=" mt-2 mb-4"> 
        <pre>pip install -r &lt;folder_to_load_file&gt;\requirements.txt </pre>
    </div>

9. Para desintalar librerías utilizando un archivo requirements.txt, el procemiento es similar a instalar archivos, usando el comando `uninstall`.

    <div class=" mt-2 mb-4"> 
        <pre>pip uninstall -r &lt;folder_to_load_file&gt;\requirements.txt </pre>
    </div>

10. Para borrar un entorno virtual es necesario desintalar las librerias instaladas, `deactivate` el venv en la terminal y borrar la carpeta donde se almacena el entorno virtual.

    <div class=" mt-2 mb-4"> 
        <pre>rm -r &lt;venv_dir&gt;/ </pre>
    </div>

Nota: también es posible ir a los directorios donde guardar entornos virtuales y archivos usando cd &lt;folder_to_active&gt; y dir (para revisar las carpetas del directorio en el que se está posicionado).

Mayor información:

- [pip documentation](https://pip.pypa.io/en/stable/installation/)
- [python docs - venv](https://docs.python.org/es/3.8/library/venv.html)