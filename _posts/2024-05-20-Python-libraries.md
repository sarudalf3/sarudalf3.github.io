---
layout: post
title:  "4 funciones útiles para programadores en Python"
categories: ['pip/Anaconda','Python','libraries']
author: rmirfa
image: assets/images/posts/py_libraries.png

---

Python contiene múltiples librerías con funciones útiles en el trabajo diario; en este artículo describo como generar datos falsos, imprimir calendarios de fecha, convertir texto en audio (text-to-speech) y como apagar el computador con una instrucción en python; las librerías utilizadas y el código de programación como ejemplo. 


1. Generación de datos falsos

*Faker* es una biblioteca de Python que genera datos falsos. Ya sea que necesites inicializar tu base de datos, crear documentos XML, completar BBDD para realizar tests o anonimizar datos obtenidos de un servicio de producción; Faker es la herramienta indicada.

```python
#!pip install Faker
from faker import Faker 
fake = Faker('es_CL') #Settings language-region
print(fake.name())
print(fake.email())
print(fake.country())
print(fake.phone_number()) 
```

La salida del código es la siguiente:

```
Luis Jara Rodríguez
escobarbastian@example.net
Rumania
+56 2 2435 0399
```
[Documentación librería Faker](https://faker.readthedocs.io/en/master/)


2. Apagado del computador

La biblioteca *os* en Python es una biblioteca incorporada que proporciona una interfaz para interactuar con el sistema operativo subyacente. Este módulo provee una manera versátil de usar funcionalidades dependientes del sistema operativo.

Permite realizar diversas operaciones relacionadas con archivos y directorios, así como obtener información del sistema, manejar variables de entorno y ejecutar comandos en la terminal.

Para apagar el equipo se utiliza la función *os.system()*, este compila instrucciones en lenguage C. En el código de ejemplo, se pide confirmación por consola el apagado del equipo

```python
import os
shutdown = input("quieres apagar el equipo (Y / n): ")
if shutdown == 'Y':
    os.system("shutdown /s /t 1")
else:
    print('El apagdo no es requerido') 
```

[Guia con funciones útiles de la libreria os](https://pythonmania.org/how-to-use-os-module-in-python/)


3. calendar

La biblioteca *calendar* de Python permite generar calendarios y proporciona funciones útiles relacionadas con configuración y cálculo de de fechas.

El siguiente ejemplo es para imprimir el calendario de un mes-año específico.


```python
#Septiembre 2020
cal = calendar.LocaleTextCalendar(locale='es_CL') #Settings language-region
print(cal.formatmonth(2020, 9))
```
La salida es la siguiente

```
  septiembre 2020
lu ma mi ju vi sá do
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30
```

En cambio, el calendario de un año completo se puede imprimir de la forma

```python
print(calendar.calendar(2024))

```

Con la siguiente salida.

```

                                  2024
      January                   February                   March
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
 1  2  3  4  5  6  7                1  2  3  4                   1  2  3
 8  9 10 11 12 13 14       5  6  7  8  9 10 11       4  5  6  7  8  9 10
15 16 17 18 19 20 21      12 13 14 15 16 17 18      11 12 13 14 15 16 17
22 23 24 25 26 27 28      19 20 21 22 23 24 25      18 19 20 21 22 23 24
29 30 31                  26 27 28 29               25 26 27 28 29 30 31

       April                      May                       June
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
 1  2  3  4  5  6  7             1  2  3  4  5                      1  2
 8  9 10 11 12 13 14       6  7  8  9 10 11 12       3  4  5  6  7  8  9
15 16 17 18 19 20 21      13 14 15 16 17 18 19      10 11 12 13 14 15 16
22 23 24 25 26 27 28      20 21 22 23 24 25 26      17 18 19 20 21 22 23
29 30                     27 28 29 30 31            24 25 26 27 28 29 30

        July                     August                  September
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
 1  2  3  4  5  6  7                1  2  3  4                         1
 8  9 10 11 12 13 14       5  6  7  8  9 10 11       2  3  4  5  6  7  8
15 16 17 18 19 20 21      12 13 14 15 16 17 18       9 10 11 12 13 14 15
22 23 24 25 26 27 28      19 20 21 22 23 24 25      16 17 18 19 20 21 22
29 30 31                  26 27 28 29 30 31         23 24 25 26 27 28 29
                                                    30

      October                   November                  December
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
    1  2  3  4  5  6                   1  2  3                         1
 7  8  9 10 11 12 13       4  5  6  7  8  9 10       2  3  4  5  6  7  8
14 15 16 17 18 19 20      11 12 13 14 15 16 17       9 10 11 12 13 14 15
21 22 23 24 25 26 27      18 19 20 21 22 23 24      16 17 18 19 20 21 22
28 29 30 31               25 26 27 28 29 30         23 24 25 26 27 28 29
                                                    30 31
```


[Guia rápida librería calendar](https://runebook.dev/en/docs/python/library/calendar)


4. pyttsx3

La biblioteca *pyttsx3* permite convertir texto en voz. A diferencia de otras bibliotecas, pyttsx3 funciona sin conexión a internet y es compatible tanto con Python 2 como con Python 3.

Hay que revisar la configuración disponible de voces instalada en el computulador local.


```python
import pyttsx3
engine = pyttsx3.init()
engine.setProperty('rate', 150) #rpm
engine.setProperty('volume',0.7) #0 to 1

voices = engine.getProperty('voices')   #getting details of current voice
engine.setProperty('voice', voices[1].id) 
engine.say('Asombrosa Programación')
engine.runAndWait() 
```

[Documentación librería pyttsx3](https://pypi.org/project/pyttsx3/)