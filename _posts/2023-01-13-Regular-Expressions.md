---
layout: post
title:  "Expresiones Regulares con Pandas: Guía Práctica"
categories: ['Pandas', 'RegEx','Python']
author: rmirfa
image: assets/images/posts/Regex.png
featured: True
---

En el análisis de datos moderno, la transformación de texto es un área fundamental. Con el auge del procesamiento de lenguaje natural (NLP) y el análisis de comportamiento, técnicas como la categorización de texto, la extracción de entidades y el análisis de sentimiento se han vuelto herramientas esenciales para obtener valor de registros no estructurados.

En este artículo, exploraremos cómo realizar búsquedas, extracciones y filtrados de patrones de texto en **DataFrames de Pandas** utilizando expresiones regulares. Si necesitas refrescar la sintaxis básica, te recomiendo este [RegEx Cheat Sheet](https://www.dataquest.io/blog/regex-cheatsheet/).

---

### 1. Librerías y Preparación

Para comenzar, necesitaremos `pandas` para la estructura de datos y el módulo nativo `re` de Python para operaciones avanzadas de expresiones regulares.

```python
import pandas as pd
import re
```

### 2. Creación del Dataset
Utilizaremos un conjunto de "palabras raras" en idioma español y sus significados para poner a prueba nuestras consultas. Estas palabras nos permitirán demostrar cómo limpiar y extraer información de cadenas de texto complejas.

```python
data = [["Acendrado","Acendrado es una palabra que puedes usar para describir algo 'puro. Sin mancha ni defecto'."], ["Ademán","Seguro habrás notado que las personas tenemos ciertos movimientos propios con los que nos expresamos. Este movimiento se llama ademán y la RAE lo define como 'movimiento o actitud del cuerpo o de alguna parte suya con que se manifiesta disposición, intención o sentimiento'."],
["Agibílibus","Esta palabra rara es un tanto complicada de pronunciar, pero una vez lo logras la vas a querer utilizar para describir a una que otra persona que conoces. Agibílibus o agilíbus es aquello que tiene una persona, la 'habilidad, ingenio, a veces pícaro, para desenvolverse en la vida'."],
["Apapachar","Apapachar es el acto de dar un apapacho a alguien, es decir, de dar una palmadita cariñosa o abrazo, según nos indica la RAE. Sin embargo, apapachar es una palabra extraña y absolutamente hermosa cuando la aprendemos bajo sus raíces del náhuatl, donde la palabra utilizada es papachoa y en su proceso de castellanización se transformó en apapacho y apapachar en el acto de 'acariciar el alma'."],
["Arrebol","Existe una palabra extraña para describir el efecto de la luz sobre las nubes. Esta es arrebol y se trata de 'cuando las nubes adquieren un color rojo al ser iluminadas por los rayos del Sol'."],
["Ataraxia","La ataraxia se utiliza para definir la 'imperturbabilidad, serenidad'."],
["Barbián","Si alguien te describe como una mujer barbiana tómalo como un cumplido, pues el significado de esta palabra no tiene nada que ver con bárbaro o con barba. Barbián es un adjetivo que se utiliza para describir a una persona que es 'desenvuelta, gallarda, arriscada'."],
["Befar","Befar significa burlarse de alguien. Es una palabra onomatopéyica, según los filólogos."],
["Bonhomía","Una palabra rara y diferente que se define como la 'afabilidad, sencillez, bondad y honradez en el carácter y en el comportamiento'. Proviene de una contracción de buen y hombre."],
["Cagaprisas","Y si, esta palabra extraña parece una malformación del lenguaje, pero sin embargo forma parte del castellano y está aceptada por la RAE. Cagaprisas, como se sobreentiende, describe a una persona que es impaciente, que siempre tiene prisa."],
["Celaje","Cuando el cielo tiene nubes de distintas texturas, formando un horizonte colorido, podemos usar la palabra celaje."],
["Conflictuar","Conflictuar hace referencia al acto de provocar un conflicto en algo o alguien. Es una de las palabras raras derivada del uso particular de otras palabras y aceptada hace pocos años por la RAE. Su significado también es el de 'sufrir un conflicto interno o preocupación que pueden llegar a condicionar su comportamiento' cuando es uno mismo el que habla de conflictuar."],
["Elocuencia","Elocuencia describe 'el arte de hablar de modo eficaz para deleitar o conmover'. Mientras vayas utilizando e integrando esta lista de palabras raras, alguien te va a poder describir como una chica elocuente."],
["Epifanía","Es lo que nos pasa cuando tenemos 'un momento de sorpresiva revelación'."],
["Euroescepticismo","Una nueva palabra que ha surgido debido a la crisis económica y política de los últimos años en europa. La RAE define esta palabra como 'la desconfianza hacia los proyectos políticos de la Unión Europea'."],
["Inefable","Esta es la palabra que tienes que utilizar cuando necesitas un adjetivo para referirte a algo que es tan increíble 'que no se puede explicar con palabras'."],
["Inmarcesible","Esta es una palabra rara que puedes utilizar cuando estás describiendo algo que no puede marchitarse."],
["Isagoge","Isagoge es una palabra extraña que podemos utilizar para referirnos a la parte del preámbulo, a la introducción, aunque probablemente nadie más que tú la va a entender."],
["Limerencia","Esta palabra rara va a encantar a las más románticas. Limerencia: estado mental involuntario, propio de la atracción romántica por parte de una persona hacia otra."],
["Melifluo","Otra palabra hermosa con la que te puedes referir a 'un sonido excesivamente dulce, suave o delicado'."],
["Mondo","Puedes utilizar esta bella palabra para describir algo que está 'limpio y libre de cosas añadidas o superfluas'."],
["Nefelibata","¿Tienes una amiga que para ti, siempre vive soñando en las nubes? Pues esta es la palabra rara para describirla: nefelibata,'dicho de una persona soñadora que no se apercibe de la realidad'."],
["Ochavo","Significa una octava parte, aunque se suele emplear para indicar que algo tiene poco valor."],
["Orate","También te enseñamos una nueva palabra para referirse a aquella persona que ha perdido el juicio: orate. La RAE la define como 'persona de poco juicio, moderación y prudencia'."],
["Perenne","Una palabra extraña para describir algo 'continuo, incesante, que no tiene intermisión'."],
["Petricor","Recuerdas el olor a tierra mojada, pues bien, petricor es una palabra rara para describir este olor. 'Es el nombre que recibe el olor que produce la lluvia al caer sobre suelos secos'."],
["Resiliencia","Esta es una de las palabras que probablemente has escuchado alguna vez pero que en realidad no conoces su significado. La resiliencia es 'la capacidad de adaptación de un ser vivo frente a un agente perturbador o un estado o situación adversos'. Una característica muy importante que podemos desarrollar las personas."],
["Sabrimiento","Es una palabra en desuso sinónima de sabor, aunque otra acepción sería la de hacer referencia a un chiste o chascarrillo."],
["Sempiterno","Un adjetivo que la RAE define como algo 'que durará para siempre; que, habiendo tenido principio, no tendrá fin'."],
["Serendipia","Algo espectacular que nos puede pasar a todas es la serendipia. Esta palabra rara es el 'hallazgo afortunado e inesperado que se produce cuando se está buscando otra cosa distinta'."],
["Sonámbulo","Otra de las palabras conocidas para algunas y no conocida por otras es sonámbulo y se utiliza para referirse a 'una persona que camina dormida'."],
["Superfluo","Para algunas una palabra extraña, para otras mucho más común; superfluo es un adjetivo para referirnos a algo no necesario, que está de más."],
["Uebos","Podrías pensar que se trata de una pésima ortografía para escribir la palabra huevos, pero lo cierto es que uebos existe y es por esto que hace parte de esta lista de palabras raras. Uebos quiere decir necesidad, cosa necesaria según la RAE, y proviene del latin opus. Cuando te refieres a la expresión manda huevos, la forma correcta de escribirlo sería manda uebos, pues proviene del latín ¡Mandat opus! Que traduce '¡La necesidad obliga!'"],
["Vagamundo","Una de las palabras raras aceptadas hace muy pocos años por la RAE que se deriva de la palabra anteriormente usada 'vagabundo'. Vagamundo describe a aquella persona ambulante, que va de un lado a otro por el mundo, errante y sin domicilio fijo."],
["Vulpino","Vulpino se usa para designar aquello que hace referencia a los zorros. Y, de hecho, la palabra latina Vulpes significa, justamente, zorro."]]

# Create the pandas DataFrame 
df = pd.DataFrame(data, columns = ["Palabra", "Significado"]) 
df.sample(5)
```

&nbsp;

<table style="font-size: 0.9rem;">
  <thead>
    <tr>
      <th style="width: 100px;">Palabra</th>
      <th>Significado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Sempiterno </code></td>
      <td>Un adjetivo que la RAE define como algo 'que durará para siempre; que, habiendo tenido principio, no tendrá fin'</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Inefable</code></td>
      <td>Esta es la palabra que tienes que utilizar cuando necesitas un adjetivo para referirte a algo que es tan increíble 'que no se puede explicar con palabras'</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Melifluo</code></td>
      <td>Otra palabra hermosa con la que te puedes referir a 'un sonido excesivamente dulce, suave o delicado'</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Celaje</code></td>
      <td>Cuando el cielo tiene nubes de distintas texturas, formando un horizonte colorido, podemos usar la palabra celaje</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Ataraxia</code></td>
      <td>La ataraxia se utiliza para definir la 'imperturbabilidad, serenidad'</td>
    </tr>
  </tbody>
</table>

### 3. Extracción de Información con RegEx
Pandas ofrece el método ```.str.extract()```, que permite crear nuevas columnas a partir de coincidencias de patrones específicos.

#### 3.1. Extraer texto de columnas del dataframe. 
Para este ejemplo se va a usar la expresión regular `r"(\'.*\')"` que extrae el texto que se encuentra entre comillas en la columna Significado

```Python
#Extract text between '' on Significado
df['Meaning'] = df['Significado'].str.extract(r"(\'.*\')", expand=False).str.strip("'")
df.sample(3)
```

&nbsp;

<table style="font-size: 0.9rem;">
  <thead>
    <tr>
      <th style="width: 100px;">Palabr</th>
      <th style="width: 450px;">Significado</th>
      <th>Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Vagamundo</code></td>
      <td>Una de las palabras raras aceptadas hace muy pocos años por la RAE que se deriva de la palabra anteriormente usada 'vagabundo'. Vagamundo describe a aquella persona ambulante, que va de un lado a otro por el mundo, errante y sin domicilio fijo</td>
      <td>vagabundo</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Befar</code></td>
      <td>Befar significa burlarse de alguien. Es una palabra onomatopéyica, según los filólogos.</td>
      <td> nan </td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Melifluo</code></td>
      <td>Otra palabra hermosa con la que te puedes referir a 'un sonido excesivamente dulce, suave o delicado'</td>
      <td> un sonido excesivamente dulce, suave o delicado </td>
    </tr>
  </tbody>
</table>

#### 3.2. Extraer prefijos de longitud fija
Si necesitamos los primeros caracteres de una columna (por ejemplo, para generar códigos o categorías rápidas), podemos usar `r'(^\w{5})'`:

```python
df['Prefijo'] = df['Palabra'].str.extract(r'(^\w{5})')
df[['Palabra','First words']].sample(3)
```

<table style="font-size: 0.9rem;">
  <thead>
    <tr>
      <th style="width: 100px;">Palabra</th>
      <th>First words</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Perenne</code></td>
      <td>Peren</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Orate</code></td>
      <td>Orate</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Superfluo</code></td>
      <td>Super</td>
    </tr>
  </tbody>
</table>

### 4. Técnicas de Filtrado Avanzado

El filtrado por patrones es donde las expresiones regulares realmente optimizan la limpieza de datos.

#### Función .str.count
Permite filtrar registros según la frecuencia de un patrón. En este caso, buscamos palabras que comiencen en **S** y terminen en **o** mediante la expresión `r'^S[\w]*o$'`:

```python
# Filtramos palabras que cumplen el patrón exacto
df_filtrado = df[df['Palabra'].str.count(r'^S[\w]*o$') > 0]
```

#### Función .str.match vs .str.contains
Es importante entender la diferencia:

`.match()`: Comprueba si el patrón coincide estrictamente desde el inicio de la cadena.

`.contains()`: Busca el patrón en cualquier posición de la cadena.

#### Ejemplo: Palabras que terminan con la letra 'e'
```python
df_termina_e = df[df['Palabra'].str.match(r'[\w]+e$') == True]
df_termina_e[['Palabra','Significado']].sample(3)
```

<table style="font-size: 0.9rem;">
  <thead>
    <tr>
      <th style="width: 120px;">Palabra</th>
      <th>Significado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Inmarcesible</code></td>
      <td>Esta es una palabra rara que puedes utilizar cuando estás describiendo algo que no puede marchitarse.</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Orate</code></td>
      <td>También te enseñamos una nueva palabra para referirse a aquella persona que ha perdido el juicio: orate. La RAE la define como 'persona de poco juicio, moderación y prudencia'.</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Inefable</code></td>
      <td>Esta es la palabra que tienes que utilizar cuando necesitas un adjetivo para referirte a algo que es tan increíble 'que no se puede explicar con palabras'.</td>
    </tr>
  </tbody>
</table>

#### 5. Caso Práctico: Búsqueda con Flags

A menudo necesitamos realizar búsquedas que ignoren la distinción entre mayúsculas y minúsculas. Para ello, utilizamos `flags=re.IGNORECASE` dentro de los métodos de Pandas:

#### Buscar palabras que contengan la letra 'v' sin importar si es Mayúscula

```python
palabras_v = df[df['Palabra'].str.contains(r'v', flags=re.IGNORECASE)]
palabras_v[['Palabra','Significado']]
```

<table style="font-size: 0.9rem;">
  <thead>
    <tr>
      <th style="width: 100px;">Palabra</th>
      <th>Significado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Ochavo</code></td>
      <td>Significa una octava parte, aunque se suele emplear para indicar que algo tiene poco valor.</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Vagamundo</code></td>
      <td>Una de las palabras raras aceptadas hace muy pocos años por la RAE que se deriva de la palabra anteriormente usada 'vagabundo'. Vagamundo describe a aquella persona ambulante, que va de un lado a otro por el mundo, errante y sin domicilio fijo.</td>
    </tr>
    <tr style="border-top: 2px solid #000;">
      <td><code>Vulpino</code></td>
      <td>Vulpino se usa para designar aquello que hace referencia a los zorros. Y, de hecho, la palabra latina Vulpes significa, justamente, zorro.</td>
    </tr>
  </tbody>
</table>

#### Conclusión
El uso de expresiones regulares en Pandas transforma procesos de limpieza de datos de horas en simples líneas de código. Desde la extracción de subcadenas hasta el filtrado condicional complejo, estas herramientas son indispensables en el flujo de trabajo de cualquier profesional de datos.

Para consultar el código completo y más ejemplos de implementación, puedes revisar [este repo github](https://github.com/sarudalf3/regEx).