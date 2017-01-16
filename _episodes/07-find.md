---
title: "Finding Things"
teaching: 15
exercises: 0
questions:
- "How can I find files?"
- "How can I find things in files?"
objectives:
- "Use `grep` to select lines from text files that match simple patterns."
- "Use `find` to find files whose names match simple patterns."
- "Use the output of one command as the command-line parameters to another command."
- "Explain what is meant by 'text' and 'binary' files, and why many common tools don't handle the latter well."
keypoints:
- "`find` finds files with specific properties that match patterns."
- "`grep` selects lines in files that match patterns."
- "`--help` is a flag supported by many bash commands, and programs that can be run from within Bash, to display more information on how to use these commands or programs."
- "`man command` displays the manual page for a given command."
- "`$(command)` inserts a command's output in place."
---

En la misma manera que muchos de nosotros ahora usamos "Google" como un
Verbo que significa "buscar", los programadores de Unix usan la
Palabra "grep".
"Grep" es una contracción de "global / regular expression / print",
Una secuencia común de operaciones en los primeros editores de texto de Unix.
También es el nombre de un programa de línea de comandos muy útil.

`Grep` encuentra e imprime líneas en archivos que coinciden con un patrón.
Para nuestros ejemplos,
Usaremos un archivo que contenga tres haikus tomados de un
1998 en la revista * Salon *. Para este conjunto de ejemplos,
Vamos a estar trabajando en el subdirectorio de escritura:

~~~
$ cd
$ cd writing
$ cat haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
~~~
{: .output}

> ## Forever, or Five Years
>
> No hemos vinculado a los haikus originales porque no parecen estar en el sitio de * Salon * más.
> Como [Jeff Rothenberg dijo] (http://www.clir.org/pubs/archives/ensuring.pdf),
> "La información digital dura para siempre --- o cinco años, lo que ocurra primero."
{: .callout}

Busquemos líneas que contengan la palabra "not":

~~~
$ grep not haiku.txt
~~~
{: .bash}

~~~
Is not the true Tao, until
"My Thesis" not found
Today it is not working
~~~
{: .output}

Aquí, `no` es el patrón que estamos buscando.
Es bastante simple:
Cada carácter alfanumérico coincide con sí mismo.
Después de que el patrón viene el nombre o los nombres de los archivos que estamos buscando.
La salida son las tres líneas del archivo que contienen las letras "not".

Vamos a probar un patrón diferente: "day".

~~~
$ grep day haiku.txt
~~~
{: .bash}

~~~
Yesterday it worked
Today it is not working
~~~
{: .output}

Esta vez,
Se emiten dos líneas que incluyen las letras "día".
Sin embargo, estas letras están contenidas en palabras más grandes.
Para restringir los partidos a las líneas que contienen la palabra "día" por sí solo,
Podemos dar `grep` con el indicador` -w`.
Esto limitará los coincidencias con los límites de las palabras.

~~~
$ grep -w day haiku.txt
~~~
{: .bash}

En este caso, no hay ninguna, así que la salida de `grep` está vacía. A veces no lo hacemos
Desea buscar una sola palabra, pero una frase. Esto también es fácil de hacer con
`Grep` poniendo la frase entre comillas.

~~~
$ grep -w "is not" haiku.txt
~~~
{: .bash}

~~~
Today it is not working
~~~
{: .output}

Ahora hemos visto que usted no tiene que tener citas alrededor de palabras simples,
Pero es útil utilizar comillas cuando se buscan varias palabras.
También ayuda a facilitar la distinción entre el término o frase de búsqueda
Y el archivo que se está buscando.
Utilizaremos cotizaciones en los ejemplos restantes.

Otra opción útil es `-n`, que numera las líneas que coinciden:

~~~
$ grep -n "it" haiku.txt
~~~
{: .bash}

~~~
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
~~~
{: .output}

Aquí, podemos ver que las líneas 5, 9 y 10 contienen las letras "it".

Podemos combinar opciones (es decir, flags) como lo hacemos con otros comandos de Unix.
Por ejemplo, vamos a encontrar las líneas que contienen la palabra "el". Podemos combinar
La opción `-w` para encontrar las líneas que contienen la palabra" the "y` -n` para numerar las líneas que coinciden:

~~~
$ grep -n -w "the" haiku.txt
~~~
{: .bash}

~~~
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

Ahora queremos usar la opción `-i` para hacer que nuestra búsqueda sea insensible a mayúsculas y minúsculas:

~~~
$ grep -n -w -i "the" haiku.txt
~~~
{: .bash}

~~~
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

Now, we want to use the `-v` option to invert our search, i.e., we want to output
The lines that do not contain the word "the".

~~~
$ grep -n -w -v "the" haiku.txt
~~~
{: .bash}

~~~
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
~~~
{: .output}

`Grep` tiene muchas otras opciones. Para averiguar lo que son, podemos escribir:
~~~
$ grep --help
~~~
{: .bash}

~~~
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated fixed strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
...        ...        ...
~~~
{: .output}

> ## Wildcards
>
> La verdadera potencia de `grep` no proviene de sus opciones; viene de
> El hecho de que los patrones pueden incluir comodines. (El nombre técnico para
> Éstas son ** expresiones regulares **, que
> Es lo que significa "re" en "grep".) Las expresiones regulares son complejas
> Y poderoso; Si quieres realizar búsquedas complejas, mira la lección
> En [nuestro sitio web] (http://v4.software-carpentry.org/regexp/index.html). Como catador, podemos
> Encontrar líneas que tienen un 'o' en la segunda posición como esto:
>
> ~~~
> $ grep -E '^.o' haiku.txt
> ~~~
> {: .bash}
>
> ~~~
> You bring fresh toner.
> Today it is not working
> Software is like that.
> ~~~
> {: .output}
>
> Utilizamos el indicador `-E` y ponemos el patrón entre comillas para evitar que el shell
> De intentar interpretarlo. (Si el patrón contenía un `*`, para
> Ejemplo, el shell intentaría expandirlo antes de ejecutar grep.)
> `^` En el patrón ancla la coincidencia al inicio de la línea. El
> Coincide con un solo carácter (como `?` En el shell), mientras que el `o`
> Coincide con un 'o' real.
{: .callout}

Mientras `grep` encuentre líneas en los archivos,
El comando `find` busca los archivos.
De nuevo,
Tiene muchas opciones;
Para mostrar cómo funcionan los más simples, utilizaremos el árbol de directorios que se muestra a continuación.

![File Tree for Find Example](../fig/find-file-tree.svg)

El directorio `writing` de Nelle contiene un archivo llamado` haiku.txt` y cuatro subdirectorios:
`Thesis` (que contiene un archivo tristemente vacío,` empty-draft.md`),
`Data` (que contiene dos archivos` one.txt` y `two.txt`),
Un directorio `tools` que contiene los programas` format` y `stats`,
Y un subdirectorio llamado `old`, con un archivo` oldtool`.

Para nuestro primer comando,
Vamos a ejecutar `find`.

~~~
$ find .
~~~
{: .bash}

~~~
.
./old
./old/.gitkeep
./data
./data/one.txt
./data/two.txt
./tools
./tools/format
./tools/old
./tools/old/oldtool
./tools/stats
./haiku.txt
./thesis
./thesis/empty-draft.md
~~~
{: .output}

Como siempre,
El `.` por sí mismo significa el directorio de trabajo actual,
Que es donde queremos que empiece nuestra búsqueda.
La salida `find` es el nombre de cada archivo ** y ** directorio
Bajo el directorio de trabajo actual.
Esto puede parecer inútil al principio, pero `find` tiene muchas opciones
Para filtrar la salida y en esta lección vamos a descubrir algunos
de ellos.

La primera opción en nuestra lista es
`-type d` que significa" cosas que son directorios ".
Bastante seguro,
La salida de `find` es los nombres de los cinco directorios en nuestro pequeño árbol
(Incluyendo `.`):

~~~
$ find . -type d
~~~
{: .bash}

~~~
./
./data
./thesis
./tools
./tools/old
~~~
{: .output}

Si cambiamos `-type d` por` -tipo f`,
Recibimos una lista de todos los archivos en su lugar:

~~~
$ find . -type f
~~~
{: .bash}

~~~
./haiku.txt
./tools/stats
./tools/old/oldtool
./tools/format
./thesis/empty-draft.md
./data/one.txt
./data/two.txt
~~~
{: .output}

Ahora intente emparejar por nombre:

~~~
$ find . -name *.txt
~~~
{: .bash}

~~~
./haiku.txt
~~~
{: .output}

Esperábamos que encontrara todos los archivos de texto,
Pero sólo imprime `./Haiku.txt`.
El problema es que el shell amplía los caracteres wildcard como `*` *antes* de ejecutar los comandos.
Dado que `*.txt` en el directorio actual se expande a `haiku.txt`,
El comando que corrimos era:

~~~
$ find . -name haiku.txt
~~~
{: .bash}

`Find` hizo lo que pedimos; Sólo pedimos la cosa equivocada.

Para conseguir lo que queremos,
Vamos a hacer lo que hicimos con `grep`:
Ponga `* .txt` en comillas simples para evitar que el shell expanda el comodín` * `.
De esta manera,
`Find` obtiene realmente el patrón` * .txt`, no el nombre de archivo expandido `haiku.txt`:

~~~
$ find . -name '*.txt'
~~~
{: .bash}

~~~
./data/one.txt
./data/two.txt
./haiku.txt
~~~
{: .output}

> ## Listado vs. Búsqueda
>
> `Ls` y` find` se pueden hacer para hacer cosas similares dadas las opciones correctas,
> Pero en circunstancias normales,
> `Ls` enumera todo lo que puede,
> While `find` busca cosas con ciertas propiedades y las muestra.
{: .callout}

Como dijimos anteriormente,
El poder de la línea de comandos reside en combinar herramientas.
Hemos visto cómo hacerlo con tuberías;
Veamos otra técnica.
Como acabamos de ver,
Encontrar -name `*.txt` nos da una lista de todos los archivos de texto en o debajo del directorio actual.
¿Cómo podemos combinar eso con `wc -l` para contar las líneas en todos esos archivos?

La forma más sencilla es poner el comando `find` dentro de` $ () `:

~~~
$ wc -l $(find . -name '*.txt')
~~~
{: .bash}

~~~
11 ./haiku.txt
300 ./data/two.txt
70 ./data/one.txt
381 total
~~~
{: .output}

Cuando el shell ejecuta este comando,
Lo primero que hace es ejecutar lo que esté dentro del `$ ()`.
Luego reemplaza la expresión `$ ()` con la salida de ese comando.
Dado que la salida de `find` es los tres nombres de fichero`. / Data / one.txt`, `. / Data / two.txt` y`. / Haiku.txt`,
El shell crea el comando:

~~~
$ wc -l ./data/one.txt ./data/two.txt ./haiku.txt
~~~
{: .bash}

Que es lo que queríamos.
Esta expansión es exactamente lo que hace el shell cuando se expanden comodines como `*` y `?`,
Pero nos permite usar cualquier comando que deseemos como nuestro propio "comodín".

Es muy común usar `find` y` grep` juntos.
El primero encuentra archivos que coinciden con un patrón;
El segundo busca líneas dentro de los archivos que coinciden con otro patrón.
Aquí, por ejemplo, podemos encontrar archivos PDB que contienen átomos de hierro
Buscando la cadena "FE" en todos los archivos `.pdb` por encima del directorio actual:

~~~
$ grep "FE" $(find .. -name '*.pdb')
~~~
{: .bash}

~~~
../data/pdb/heme.pdb:ATOM     25 FE           1      -0.924   0.535  -0.518
~~~
{: .output}

> ## Archivos binarios
>
> Nos hemos centrado exclusivamente en encontrar cosas en archivos de texto. Y si
> Sus datos se almacenan como imágenes, en bases de datos, o en algún otro formato?
> Una opción sería extender herramientas como `grep` para manejar esos formatos.
> Esto no ha sucedido, y probablemente no lo hará, porque hay demasiados
> Formatos a apoyar.
>
> La segunda opción es convertir los datos en texto, o extraer los
> Bits de texto de los datos. Este es probablemente el enfoque más común,
> Ya que sólo requiere que las personas construyan una herramienta por formato de datos (para
> Extraer información). Por un lado, facilita las cosas simples
Hacer En el lado negativo, las cosas complejas son generalmente imposibles. por
> Ejemplo, es bastante fácil escribir un programa que extraerá X e Y
> Dimensiones de los archivos de imagen para `grep` con los que jugar, pero ¿cómo
> Escribir algo para encontrar valores en una hoja de cálculo cuyas celdas contenían
> Fórmulas?
>
> La tercera opción es reconocer que el shell y el procesamiento de texto
> Sus límites, y utilizar otro lenguaje de programación.
> Cuando llegue el momento de hacerlo, no seas demasiado duro en la concha: muchos
> Lenguajes de programación modernos han prestado mucho
> Ideas de ella, y la imitación es también la forma más sincera de alabanza.
{: .callout}

El shell de Unix es más antiguo que la mayoría de las personas que lo usan. Tiene
Sobrevivió tanto tiempo porque es una de las programaciones más productivas
Ambientes jamás creados --- quizás incluso * los * más productivos. Su sintaxis
Puede ser críptico, pero las personas que lo han dominado pueden experimentar con
Diferentes comandos interactivamente, luego utilice lo que han aprendido para
Automatizar su trabajo. Las interfaces gráficas de usuario pueden ser
Primero, pero la concha sigue invicto en el segundo. Y como Alfred
North Whitehead escribió en 1911, "La civilización avanza extendiendo el
Número de operaciones importantes que podemos realizar sin pensar
a cerca de ellos."

> ## Using `grep`
>
> Referring to `haiku.txt`
> presented at the begin of this topic,
> which command would result in the following output:
>
> ~~~
> and the presence of absence:
> ~~~
> {: .output}
>
> 1. `grep "of" haiku.txt`
> 2. `grep -E "of" haiku.txt`
> 3. `grep -w "of" haiku.txt`
> 4. `grep -i "of" haiku.txt`
{: .challenge}

> ## `find` Pipeline Reading Comprehension
>
> Write a short explanatory comment for the following shell script:
>
> ~~~
> wc -l $(find . -name '*.dat') | sort -n
> ~~~
> {: .bash}
{: .challenge}

> ## Matching and Subtracting
>
> The `-v` flag to `grep` inverts pattern matching, so that only lines
> which do *not* match the pattern are printed. Given that, which of
> the following commands will find all files in `/data` whose names
> end in `ose.dat` (e.g., `sucrose.dat` or `maltose.dat`), but do
> *not* contain the word `temp`?
>
> 1.  `find /data -name '*.dat' | grep ose | grep -v temp`
> 2.  `find /data -name ose.dat | grep -v temp`
> 3.  `grep -v "temp" $(find /data -name '*ose.dat')`
> 4.  None of the above.
{: .challenge}

> ## Tracking a Species
> 
> Leah has several hundred 
> data files saved in one directory, each of which is formatted like this:
> 
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> ~~~
> {: .source}
>
> She wants to write a shell script that takes a directory and a species 
> as command-line parameters and return one file called `species.txt` 
> containing a list of dates and the number of that species seen on that date,
> such as this file for rabbits:
> 
> ~~~
> 2013-11-05,22
> 2013-11-06,19
> 2013-11-07,18
> ~~~
> {: .source}
>
> Put these commands and pipes in the right order to achieve this:
> 
> ~~~
> cut -d : -f 2  
> >  
> |  
> grep -w $1 -r $2  
> |  
> $1.txt  
> cut -d , -f 1,3  
> ~~~
> {: .bash}
>
> Hint: use `man grep` to look for how to grep text recursively in a directory
> and `man cut` to select more than one field in a line.
{: .challenge}

> ## Little Women
>
> You and your friend, having just finished reading *Little Women* by
> Louisa May Alcott, are in an argument.  Of the four sisters in the
> book, Jo, Meg, Beth, and Amy, your friend thinks that Jo was the
> most mentioned.  You, however, are certain it was Amy.  Luckily, you
> have a file `LittleWomen.txt` containing the full text of the novel.
> Using a `for` loop, how would you tabulate the number of times each
> of the four sisters is mentioned?
>
> Hint: one solution might employ
> the commands `grep` and `wc` and a `|`, while another might utilize
> `grep` options.
{: .challenge}

> ## Finding Files With Different Properties
> 
> The `find` command can be given several other criteria known as "tests"
> to locate files with specific attributes, such as creation time, size,
> permissions, or ownership.  Use `man find` to explore these, and then
> write a single command to find all files in or below the current directory
> that were modified by the user `ahmed` in the last 24 hours.
>
> Hint 1: you will need to use three tests: `-type`, `-mtime`, and `-user`.
>
> Hint 2: The value for `-mtime` will need to be negative---why?
>
> Solution: Assuming that Nelle’s home is our working directory we type:
>
> ~~~
> $ find ./ -type f -mtime -1 -user ahmed
> ~~~
> {: .bash}
{: .challenge}
