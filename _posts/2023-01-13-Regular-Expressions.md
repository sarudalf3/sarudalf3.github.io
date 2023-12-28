---
layout: post
title:  "Regular Expressions usando Pandas"
categories: ['Pandas', 'RegEx','Python']
author: rmirfa
image: assets/images/posts/Regex.png
---

Un área cada vez más explotada es el análisis de datos es la transformación de registros de texto, que en el estudio de comportamiento social es cada vez más utilizado, dado el desarrollo de técnicas de transformación y modelación (text categorization, text clustering, entity extraction, taxonomies, sentiment analysis, document summarization, and entity-relation modeling,...).

En este artículo vamos a revisar usos de busquedas  y filtro de patrones de texto en pandas Dataframes. Para más detalle del uso de expresiones regulares revisar el cheat sheet de expresiones regulares [RegEx Cheat Sheet ](https://www.dataquest.io/blog/regex-cheatsheet/).

1. Librerias a utilizar

```python
import pandas as pd
import re
```

2. Ingreso de datos

Se ingresan algunas palabras en español, y su significado

```python
data = [
["Acendrado","Acendrado es una palabra que puedes usar para describir algo 'puro. Sin mancha ni defecto'."],
["Ademán","Seguro habrás notado que las personas tenemos ciertos movimientos propios con los que nos expresamos. Este movimiento se llama ademán y la RAE lo define como 'movimiento o actitud del cuerpo o de alguna parte suya con que se manifiesta disposición, intención o sentimiento'."],
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
["Vulpino","Vulpino se usa para designar aquello que hace referencia a los zorros. Y, de hecho, la palabra latina Vulpes significa, justamente, zorro."]
]

# Create the pandas DataFrame 
df = pd.DataFrame(data, columns = ["Palabra", "Significado"]) 
df.sample(5)

```

| Palabra      | Significado |
|:-------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sempiterno   | Un adjetivo que la RAE define como algo 'que durará para siempre; que, habiendo tenido principio, no tendrá fin'.|
| Inefable  | Esta es la palabra que tienes que utilizar cuando necesitas un adjetivo para referirte a algo que es tan increíble 'que no se puede explicar con palabras'. |
| Melifluo  | Otra palabra hermosa con la que te puedes referir a 'un sonido excesivamente dulce, suave o delicado'. |
| Celaje    | Cuando el cielo tiene nubes de distintas texturas, formando un horizonte colorido, podemos usar la palabra celaje. | 
| Ataraxia     | La ataraxia se utiliza para definir la 'imperturbabilidad, serenidad'. |

3. Extraer texto de columnas del dataframe. Para este ejemplo se va a usar la expresión regular `r"(\'.*\')"` que extrae el texto que se encuentra entre comillas en la columna Significado

```Python
#Extract text between '' on Significado
df['Meaning'] = df['Significado'].str.extract(r"(\'.*\')", expand=False).str.strip("'")
df.sample(5)
```

| Palabra          | Significado | Meaning  |
|:-----------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------|
| Inmarcesible     | Esta es una palabra rara que puedes utilizar cuando estás describiendo algo que no puede marchitarse.  | nan  |
| Vagamundo        | Una de las palabras raras aceptadas hace muy pocos años por la RAE que se deriva de la palabra anteriormente usada 'vagabundo'. Vagamundo describe a aquella persona ambulante, que va de un lado a otro por el mundo, errante y sin domicilio fijo.  | vagabundo |
| Befar            | Befar significa burlarse de alguien. Es una palabra onomatopéyica, según los filólogos. | nan |
| Ademán           | Seguro habrás notado que las personas tenemos ciertos movimientos propios con los que nos expresamos. Este movimiento se llama ademán y la RAE lo define como 'movimiento o actitud del cuerpo o de alguna parte suya con que se manifiesta disposición, intención o sentimiento'. | movimiento o actitud del cuerpo o de alguna parte suya con que se manifiesta disposición, intención o sentimiento |
| Euroescepticismo | Una nueva palabra que ha surgido debido a la crisis económica y política de los últimos años en europa. La RAE define esta palabra como 'la desconfianza hacia los proyectos políticos de la Unión Europea'.  | la desconfianza hacia los proyectos políticos de la Unión Europea |

Otro ejemplo es la extracción de tamaños fijos de texto. En este caso se va a usar la expresión `r'(^\w{5})'` que va a extraer las primeras cinco letras de la columna Palabras.

```python
df['First words'] = df['Palabra'].str.extract(r'(^\w{5})')
df[['Palabra','First words']].sample(5)
```

| Palabra    | First words   |
|:-----------|:--------------|
| Perenne    | Peren         |
| Agibílibus | Agibí         |
| Ademán     | Ademá         |
| Orate      | Orate         |
| Superfluo  | Super         |

4. Filtros por texto. Hay una variedad de formas de realizarlo, vamos a revsar las más utilizadas.

-  Función `count`: En este caso cuenta la frecuencia que se encuentra la expresión regular dentro del texto; en caso que no se encuentre el patrón de la expresión, este valor es 0. Se va a utilizar la expresión `r'^S[\w]*o$')`, que identifica los textos que empiezan con `S` y terminan con `o`.

```python
df[df['Palabra'].str.count(r'^S[\w]*o$')>0][['Palabra','Meaning']]
```

| Palabra     | Meaning |
|:------------|:-----------------------------------------------------------------------|
| Sabrimiento | nan |
| Sempiterno  | que durará para siempre; que, habiendo tenido principio, no tendrá fin |
| Sonámbulo   | una persona que camina dormida  |
| Superfluo   | nan  |

- Función `match`. Funciona similar a `count`, aunque su salida es Boolean, identificando si encuentró el patrón de la expresión regular en la columna. En este ejemplo se va usar la expresión `r'[\w]+e$'`, y equivale a todas las palabras que terminen con la letra `e`.

```python
df[df['Palabra'].str.match(r'[\w]+e$')==True][['Palabra','Meaning']]
```

| Palabra      | Meaning                                        |
|:-------------|:-----------------------------------------------|
| Celaje       | nan                                            |
| Inefable     | que no se puede explicar con palabras          |
| Inmarcesible | nan                                            |
| Isagoge      | nan                                            |
| Orate        | persona de poco juicio, moderación y prudencia |
| Perenne      | continuo, incesante, que no tiene intermisión  |

- Función `findall` (útil en listas). En este caso entrega todas los patrones encontrados de la expresión regular (siendo en la misma palabra), almacenandolos en una lista. En este caso transformamos en lista las palabras, para obtener sólo las que tienen la palabra `v` y equivale a la expresión `r'(\w*v\w*)'`, indifirente si es mayúscula o minúscula `re.IGNORECASE`

```python
palabras = pd.Series(df['Palabra'])
palabras = [itm[0] for itm in palabras.str.findall(r'(\w*v\w*)', flags=re.IGNORECASE) if len(itm)>0]
df[df['Palabra'].isin(palabras)][['Palabra','Significado']]
```

| Palabra   | Significado   |
|:----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ochavo    | Significa una octava parte, aunque se suele emplear para indicar que algo tiene poco valor. |
| Vagamundo | Una de las palabras raras aceptadas hace muy pocos años por la RAE que se deriva de la palabra anteriormente usada 'vagabundo'. Vagamundo describe a aquella persona ambulante, que va de un lado a otro por el mundo, errante y sin domicilio fijo. |
| Vulpino   | Vulpino se usa para designar aquello que hace referencia a los zorros. Y, de hecho, la palabra latina Vulpes significa, justamente, zorro. |

- Función `contains`. Esta función identifica los registros que contienen la expresión regular `r'^I.*e$'`, en este caso la palabra empieza con `I` y termina con `e`. Es una función booleana, donde es True cuando encuentra el patrón.

```python
df[df['Palabra'].str.contains(r'^I.*e$')==True]
```

| Palabra      | Significado |
|:-------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inefable     | Esta es la palabra que tienes que utilizar cuando necesitas un adjetivo para referirte a algo que es tan increíble 'que no se puede explicar con palabras'. |
| Inmarcesible | Esta es una palabra rara que puedes utilizar cuando estás describiendo algo que no puede marchitarse.  |
| Isagoge      | Isagoge es una palabra extraña que podemos utilizar para referirnos a la parte del preámbulo, a la introducción, aunque probablemente nadie más que tú la va a entender. |


Para mayor información, revisar [este repositorio github](https://github.com/sarudalf3/regEx).