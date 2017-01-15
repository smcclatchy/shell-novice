---
title: "Pipes and Filters"
teaching: 15
exercises: 0
questions:
- "How can I combine existing commands to do new things?"
objectives:
- "Redirect a command's output to a file."
- "Process a file instead of keyboard input using redirection."
- "Construct command pipelines with two or more stages."
- "Explain what usually happens if a program or pipeline isn't given any input to process."
- "Explain Unix's 'small pieces, loosely joined' philosophy."
keypoints:
- "`cat` displays the contents of its inputs."
- "`head` displays the first few lines of its input."
- "`tail` displays the last few lines of its input."
- "`sort` sorts its inputs."
- "`wc` counts lines, words, and characters in its inputs."
- "`*` matches zero or more characters in a filename, so `*.txt` matches all files ending in `.txt`."
- "`?` matches any single character in a filename, so `?.txt` matches `a.txt` but not `any.txt`."
- "`command > file` redirects a command's output to a file."
- "`first | second` is a pipeline: the output of the first command is used as the input to the second."
- "The best way to use the shell is to use pipes to combine simple single-purpose programs (filters)."
---

Ahora que sabemos algunos comandos básicos,
Finalmente podemos ver la característica más poderosa de la shell:
La facilidad con la que nos permite combinar los programas existentes de nuevas maneras.
Comenzaremos con un directorio llamado `moléculas`
Que contiene seis archivos que describen algunas moléculas orgánicas simples.
La extensión `.pdb` indica que estos archivos están en formato Protein Data Bank,
Un formato de texto simple que especifica el tipo y la posición de cada átomo en la molécula.

~~~
$ ls molecules
~~~
{: .bash}

~~~
cubane.pdb    ethane.pdb    methane.pdb
octane.pdb    pentane.pdb   propane.pdb
~~~
{: .output}

Vamos a entrar en ese directorio con `cd` y ejecutar el comando` wc * .pdb`.
`Wc` es el comando" word count ":
Cuenta el número de líneas, palabras y caracteres en los archivos.
El `*` en `* .pdb` coincide con cero o más caracteres,
Así que el shell convierte `* .pdb` en una lista de todos los archivos` .pdb` en el directorio actual:

~~~
$ cd molecules
$ wc *.pdb
~~~
{: .bash}

~~~
  20  156 1158 cubane.pdb
  12   84  622 ethane.pdb
   9   57  422 methane.pdb
  30  246 1828 octane.pdb
  21  165 1226 pentane.pdb
  15  111  825 propane.pdb
 107  819 6081 total
~~~
{: .output}

> ## comodines
>
> `*` Es un comodín **. Corresponde a cero o más
>, Así que `* .pdb` coincide con` ethane.pdb`, `propane.pdb`, y cada
> Archivo que termina con '.pdb'. Por otro lado, `p * .pdb` sólo coincide
> `Pentane.pdb` y` propane.pdb`, porque el 'p' sólo en la parte delantera
> Hace coincidir los nombres de archivo que comienzan con la letra 'p'.
>
> `?` También es un comodín, pero sólo coincide con un solo carácter. Esto
> Significa que `p? .pdb` podría coincidir con` pi.pdb` o `p5.pdb` (si tuviéramos estos dos
> En el directorio `molecules`), pero no` propane.pdb`.
> Podemos usar cualquier número de comodines a la vez: por ejemplo, `p * .p? *`
> Coincide con cualquier cosa que comience con un 'p' y termine con '.', 'P', y en
> Al menos un carácter más (ya que `?` Tiene que coincidir con un carácter, y
> El final `*` puede coincidir con cualquier número de caracteres). Por lo tanto, `p * .p? *`
> Match `preferred.practice`, e incluso` p.pi` (desde el primer `*` puede
> No coinciden con ningún carácter), pero no `quality.practice` (no se inicia
> Con 'p') o `preferred.p` (no hay al menos un carácter después del
> '.p').
>
> Cuando el shell ve un comodín, expande el comodín para crear un
> Lista de nombres de archivo coincidentes * antes * ejecutando el comando que fue
> Pidió. Como excepción, si una expresión comodín no coincide
> Cualquier archivo, Bash pasará la expresión como un parámetro al comando
> Tal como es. Por ejemplo, escribiendo `ls * .pdf` en el directorio` molecules`
> (Que contiene sólo los archivos con nombres que terminan con `.pdb`) da como resultado
> Un mensaje de error que no hay ningún archivo llamado `* .pdf`.
> Sin embargo, generalmente los comandos como `wc` y` ls` ver las listas de
> Nombres de archivo que coinciden con estas expresiones, pero no los comodines
> Ellos mismos. Es la cáscara, no los otros programas, que se ocupa de
> Expansión de comodines, y este es otro ejemplo de diseño ortogonal.
{: .callout}

> ## Using Wildcards
>
> When run in the `molecules` directory, which `ls` command(s) will
> produce this output?
>
> `ethane.pdb   methane.pdb`
>
> 1. `ls *t*ane.pdb`
> 2. `ls *t?ne.*`
> 3. `ls *t??ne.pdb`
> 4. `ls ethane.*`
{: .challenge}

Si ejecutamos `wc -l` en lugar de` wc`,
La salida sólo muestra el número de líneas por archivo:

~~~
$ wc -l *.pdb
~~~
{: .bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

También podemos usar `-w` para obtener sólo el número de palabras,
O `-c` para obtener sólo el número de caracteres.

¿Cuál de estos archivos es el más corto?
Es una pregunta fácil de responder cuando sólo hay seis archivos,
Pero ¿y si había 6000?
Nuestro primer paso hacia una solución es ejecutar el comando:

~~~
$ wc -l *.pdb > lengths.txt
~~~
{: .bash}

El mayor que el símbolo, `>`, le dice a la shell que ** redireccione ** la salida del comando
A un archivo en lugar de imprimirlo a la pantalla. (Es por eso que no hay salida de pantalla:
Todo lo que `wc` habría impreso ha entrado en el
Archivo `lengths.txt` en su lugar.) El shell creará
El archivo si no existe. Si el archivo existe, será
Sobrescrita en silencio, lo que puede conducir a la pérdida de datos y
Cierta precaución.
`Ls lengths.txt` confirma que el archivo existe:

~~~
$ ls lengths.txt
~~~
{: .bash}

~~~
lengths.txt
~~~
{: .output}

Ahora podemos enviar el contenido de `lengths.txt` a la pantalla usando` cat lengths.txt`.
`Cat` significa" concatenate ":
Imprime el contenido de los archivos uno tras otro.
Sólo hay un archivo en este caso,
Así `cat` sólo nos muestra lo que contiene:

~~~
$ cat lengths.txt
~~~
{: .bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

> ## Salida Página por página
>
> Continuaremos usando `cat` en esta lección, por conveniencia y consistencia,
> Pero tiene la desventaja de que siempre vuelca todo el archivo en la pantalla.
> Más útil en la práctica es el comando `less`,
> Que usas con `$ less lengths.txt`.
> Esto muestra una pantalla del archivo y luego se detiene.
> Puede avanzar una pantalla presionando la barra espaciadora,
> O retroceda presionando `b`. Pulse `q` para salir.
{: .callout}

Ahora vamos a usar el comando `sort` para ordenar su contenido.
También usaremos el indicador `-n` para especificar que el tipo es
Numérico en lugar de alfabético.
Esto * no * cambia el archivo;
En su lugar, envía el resultado ordenado a la pantalla:

~~~
$ sort -n lengths.txt
~~~
{: .bash}

~~~
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
~~~
{: .output}

Podemos poner la lista ordenada de líneas en otro archivo temporal llamado `sorted-lengths.txt`
Poniendo `> sorted-lengths.txt` después del comando,
Así como usamos `> lengths.txt` para poner la salida de` wc` en `lengths.txt`.
Una vez que hayamos hecho eso,
Podemos ejecutar otro comando llamado `head` para obtener las primeras líneas en` sorted-lengths.txt`:

~~~
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
~~~
{: .bash}

~~~
  9  methane.pdb
~~~
{: .output}

El uso del parámetro `-n 1` con` head` indica que
Sólo queremos la primera línea del archivo;
`-n 20` conseguiría los primeros 20,
y así.
Dado que `sorted-lengths.txt` contiene las longitudes de nuestros archivos ordenados de menor a mayor,
La salida de `head` debe ser el archivo con menos líneas.

> ## Redirigiendo al mismo archivo
>
> Es una mala idea intentar redireccionar
> La salida de un comando que opera en un archivo
> Al mismo archivo. Por ejemplo:
>
> ~~~
> $ sort -n lengths.txt > lengths.txt
> ~~~
> {: .bash}
>
> Hacer algo como esto puede darle
> Resultados incorrectos y / o eliminar
> El contenido de `lengths.txt`.
{: .callout}

Si crees que esto es confuso,
Usted está en buena compañía:
Incluso una vez que entienda lo que `wc`,` sort` y `head`,
Todos esos archivos intermedios hacen difícil seguir lo que está pasando.
Podemos hacerlo más fácil de entender ejecutando `sort` y` head` juntos:

~~~
$ sort -n lengths.txt | head -n 1
~~~
{: .bash}

~~~
  9  methane.pdb
~~~
{: .output}

La barra vertical, `|`, entre los dos comandos se denomina un **pipe**.
Le dice a la shell que queremos usar
La salida del comando a la izquierda
Como entrada al comando de la derecha.
El equipo puede crear un archivo temporal si es necesario,
O copiar datos de un programa a otro en la memoria,
O algo más completamente;
No tenemos que saber o cuidar.

Nada nos impide encadenar pipes consecutivamente.
Es decir, podemos por ejemplo enviar la salida de `wc` directamente a` sort`,
Y luego la salida resultante a `head`.
Así, primero usamos un tubo para enviar la salida de `wc` a` sort`:

~~~
$ wc -l *.pdb | sort -n
~~~
{: .bash}

~~~
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
~~~
{: .output}

Y ahora enviamos la salida de este pipe, a través de otro pipe, a `head`, para que el pipe completo se convierta en:

~~~
$ wc -l *.pdb | sort -n | head -n 1
~~~
{: .bash}

~~~
   9  methane.pdb
~~~
{: .output}

Esto es exactamente como un matemático anidando funciones como * log (3x) *
Y diciendo "el registro de tres veces * x *".
En nuestro caso,
El cálculo es "head of sort of line count of `*.pdb` ".

Esto es lo que realmente sucede detrás de las escenas cuando creamos un tubo.
Cuando una computadora ejecuta un programa --- cualquier programa --- crea un ** proceso **
En memoria para almacenar el software del programa y su estado actual.
Cada proceso tiene un canal de entrada llamado ** entrada estándar **.
(Por este punto, puede ser sorprendido que el nombre es tan memorable, pero no te preocupes:
La mayoría de los programadores de Unix lo llaman "stdin").
Cada proceso también tiene un canal de salida predeterminado llamado ** salida estándar **
(O "stdout"). Un tercer canal de salida llamado ** error estándar ** (stderr) también
Existe. Este canal suele utilizarse para mensajes de error o de diagnóstico y
Permite al usuario canalizar la salida de un programa a otro mientras sigue recibiendo
Mensajes de error en el terminal.

El shell es realmente apenas otro programa.
Bajo circunstancias normales,
Lo que tecleamos en el teclado se envía a la shell en su entrada estándar,
Y lo que produce en la salida estándar se muestra en nuestra pantalla.
Cuando le decimos al shell que ejecute un programa,
Crea un nuevo proceso
Y envía temporalmente lo que tecleamos en nuestro teclado a la entrada estándar de ese proceso,
Y lo que el proceso envía a la salida estándar a la pantalla.

Esto es lo que ocurre cuando ejecutamos `wc -l *.pdb > lengths.txt`.
El shell comienza diciéndole a la computadora que cree un nuevo proceso para ejecutar el programa `wc`.
Como hemos proporcionado algunos nombres de archivo como parámetros,
`Wc` lee de ellos en vez de la entrada estándar.
Y puesto que hemos utilizado `>` para redirigir la salida a un archivo,
El shell conecta la salida estándar del proceso a ese archivo.

Si ejecutamos `wc -l * .pdb | Ordenar -n` en su lugar,
El shell crea dos procesos
(Uno para cada proceso en el pipe)
De modo que `wc` y` sort` funcionen simultáneamente.
La salida estándar de `wc` se alimenta directamente a la entrada estándar de `sort`;
Ya que no hay redirección con `>`,
La salida `sort` va a la pantalla.
Y si ejecutamos `wc -l * .pdb | sort -n | head -n 1`,
Obtenemos tres procesos con datos que fluyen de los archivos,
A través de `wc` a` sort`,
Y de `sort` a` head` a la pantalla.

![Redirects and Pipes](../fig/redirects-and-pipes.png)

Esta sencilla idea es por qué Unix ha tenido tanto éxito.
En lugar de crear enormes programas que tratan de hacer muchas cosas diferentes,
Los programadores de Unix se centran en crear muchas herramientas simples que cada uno haga un trabajo bien,
Y que funcionan bien entre sí.
Este modelo de programación se llama "pipes y filters".
Ya hemos visto pipes;
Un ** filtro ** es un programa como `wc` o` sort`
Que transforma una corriente de entrada en una corriente de salida.
Casi todas las herramientas Unix estándar pueden funcionar de esta manera:
A menos que se le indique que haga lo contrario,
Que leen de entrada estándar,
Hacer algo con lo que han leído,
Y escribir en la salida estándar.

La clave es que cualquier programa que lea líneas de texto de entrada estándar
Y escribe líneas de texto en la salida estándar
Puede combinarse con cualquier otro programa que se comporte de esta manera también.
Usted puede * y debe * escribir sus programas de esta manera
Para que usted y otras personas puedan poner esos programas en pipes para multiplicar su poder.

> ## Redireccionamiento de entrada
>
> Además de usar `>` para redirigir la salida de un programa, podemos usar `<` to
> Redirigir su entrada, es decir, para leer un archivo en lugar de un archivo estándar
> Entrada. Por ejemplo, en lugar de escribir `wc ammonia.pdb`, podríamos escribir
> `Wc <ammonia.pdb`. En el primer caso, `wc` obtiene una línea de comandos
> Parámetro diciéndole qué archivo abrir. En el segundo, `wc` no tiene
> Cualquier parámetro de la línea de comandos, por lo que se lee desde la entrada estándar, pero
> Han dicho al shell que envíe el contenido de `ammonia.pdb` a` wc`
> Entrada estándar.
{: .callout}

## Nelle's Pipeline: Checking Files

Nelle ha corrido sus muestras a través de las máquinas de ensayo
Y creó 1520 archivos en el directorio `north-pacific-gyre / 2012-07-03` descrito anteriormente.
Como un control de cordura rápida, a partir de su directorio de inicio, los tipos Nelle:

~~~
$ cd north-pacific-gyre/2012-07-03
$ wc -l *.txt
~~~
{: .bash}

La salida es 1520 líneas que se parecen a esto:

~~~
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
~~~
{: .output}

Ahora escribe esto:

~~~
$ wc -l *.txt | sort -n | head -n 5
~~~
{: .bash}

~~~
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
~~~
{: .output}

Whoops: uno de los archivos es 60 líneas más corto que los otros.
Cuando ella vuelve y lo comprueba,
Ella ve que hizo ese ensayo a las 8:00 un lunes por la mañana --- alguien
Fue probablemente en el uso de la máquina en el fin de semana,
Y se olvidó de restablecerlo.
Antes de volver a ejecutar esa muestra,
Ella comprueba si algunos archivos tienen demasiados datos:

~~~
$ wc -l *.txt | sort -n | tail -n 5
~~~
{: .bash}

~~~
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
~~~
{: .output}

Estes numeros parecen buenos --- pero ¿qué es ese 'Z' haciendo allí en la línea de tercero a la última?
Todas sus muestras deben estar marcadas con "A" o "B";
por convención,
Su laboratorio utiliza 'Z' para indicar muestras con información que falta.
Para encontrar a otras personas como ella, ella hace esto:

~~~
$ ls *Z.txt
~~~
{: .bash}

~~~
NENE01971Z.txt    NENE02040Z.txt
~~~
{: .output}

Cuando ella comprueba el registro en su computadora portátil,
No hay profundidad registrada para ninguna de esas muestras.
Ya que es demasiado tarde para obtener la información de otra manera,
Ella debe excluir esos dos archivos de su análisis.
Podría simplemente borrarlos usando `rm`,
Pero en realidad hay algunos análisis que podría hacer más tarde, donde la profundidad no importa,
Por lo que en su lugar, sólo tendrá cuidado más adelante para seleccionar archivos utilizando la expresión comodín `* [AB] .txt`.
Como siempre,
El `*` coincide con cualquier número de caracteres;
La expresión «[AB]» coincide con una «A» o una «B»,
Por lo que coincide con todos los archivos de datos válidos que tiene.

> ## What Does `sort -n` Do?
>
> If we run `sort` on this file:
>
> ~~~
> 10
> 2
> 19
> 22
> 6
> ~~~
> {: .source}
>
> the output is:
>
> ~~~
> 10
> 19
> 2
> 22
> 6
> ~~~
> {: .output}
>
> If we run `sort -n` on the same input, we get this instead:
>
> ~~~
> 2
> 6
> 10
> 19
> 22
> ~~~
> {: .output}
>
> Explain why `-n` has this effect.
{: .challenge}

> ## What Does `<` Mean?
>
> What is the difference between:
>
> ~~~
> $ wc -l < mydata.dat
> ~~~
> {: .bash}
>
> and:
>
> ~~~
> $ wc -l mydata.dat
> ~~~
> {: .bash}
{: .challenge}

> ## What Does `>>` Mean?
>
> What is the difference between:
>
> ~~~
> $ echo hello > testfile01.txt
> ~~~
> {: .bash}
>
> and:
>
> ~~~
> $ echo hello >> testfile02.txt
> ~~~
> {: .bash}
>
> Hint: Try executing each command twice in a row and then examining the output files.
{: .challenge}

> ## More on Wildcards
>
> Sam has a directory containing calibration data, datasets, and descriptions of
> the datasets:
>
> ~~~
> 2015-10-23-calibration.txt
> 2015-10-23-dataset1.txt
> 2015-10-23-dataset2.txt
> 2015-10-23-dataset_overview.txt
> 2015-10-26-calibration.txt
> 2015-10-26-dataset1.txt
> 2015-10-26-dataset2.txt
> 2015-10-26-dataset_overview.txt
> 2015-11-23-calibration.txt
> 2015-11-23-dataset1.txt
> 2015-11-23-dataset2.txt
> 2015-11-23-dataset_overview.txt
> ~~~
> {: .bash}
>
> Before heading off to another field trip, she wants to back up her data and
> send some datasets to her colleague Bob. Sam uses the following commands
> to get the job done:
>
> ~~~
> $ cp *dataset* /backup/datasets
> $ cp ____calibration____ /backup/calibration
> $ cp 2015-____-____ ~/send_to_bob/all_november_files/
> $ cp ____ ~/send_to_bob/all_datasets_created_on_a_23rd/
> ~~~
> {: .bash}
>
> Help Sam by filling in the blanks.
{: .challenge}

> ## Piping Commands Together
>
> In our current directory, we want to find the 3 files which have the least number of
> lines. Which command listed below would work?
>
> 1. `wc -l * > sort -n > head -n 3`
> 2. `wc -l * | sort -n | head -n 1-3`
> 3. `wc -l * | head -n 3 | sort -n`
> 4. `wc -l * | sort -n | head -n 3`
{: .challenge}

> ## Why Does `uniq` Only Remove Adjacent Duplicates?
>
> The command `uniq` removes adjacent duplicated lines from its input.
> For example, if a file `salmon.txt` contains:
>
> ~~~
> coho
> coho
> steelhead
> coho
> steelhead
> steelhead
> ~~~
> {: .source}
>
> then `uniq salmon.txt` produces:
>
> ~~~
> coho
> steelhead
> coho
> steelhead
> ~~~
> {: .output}
>
> Why do you think `uniq` only removes *adjacent* duplicated lines?
> (Hint: think about very large data sets.) What other command could
> you combine with it in a pipe to remove all duplicated lines?
{: .challenge}

> ## Pipe Reading Comprehension
>
> A file called `animals.txt` contains the following data:
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> 2012-11-06,deer
> 2012-11-06,fox
> 2012-11-07,rabbit
> 2012-11-07,bear
> ~~~
> {: .source}
>
> What text passes through each of the pipes and the final redirect in the pipeline below?
>
> ~~~
> $ cat animals.txt | head -n 5 | tail -n 3 | sort -r > final.txt
> ~~~
> {: .bash}
{: .challenge}

> ## Pipe Construction
>
> For the file `animals.txt` from the previous exercise, the command:
>
> ~~~
> $ cut -d , -f 2 animals.txt
> ~~~
> {: .bash}
>
> produces the following output:
>
> ~~~
> deer
> rabbit
> raccoon
> rabbit
> deer
> fox
> rabbit
> bear
> ~~~
> {: .output}
>
> What other command(s) could be added to this in a pipeline to find
> out what animals the file contains (without any duplicates in their
> names)?
{: .challenge}

> ## Removing Unneeded Files
>
> Suppose you want to delete your processed data files, and only keep
> your raw files and processing script to save storage.
> The raw files end in `.dat` and the processed files end in `.txt`.
> Which of the following would remove all the processed data files,
> and *only* the processed data files?
>
> 1. `rm ?.txt`
> 2. `rm *.txt`
> 3. `rm * .txt`
> 4. `rm *.*`
{: .challenge}

> ## Wildcard Expressions
>
> Wildcard expressions can be very complex, but you can sometimes write
> them in ways that only use simple syntax, at the expense of being a bit
> more verbose.  For example, the wildcard expression `*[AB].txt`
> matches all files ending in `A.txt` or `B.txt`. Imagine you forgot about
> this.
>
> 1.  Can you match the same set of files with basic wildcard expressions
>     that do not use the `[]` syntax? *Hint*: You may need more than one
>     expression.
>
> 2.  The expression that you found and the expression from the lesson match the
>     same set of files in this example. What is the small difference between the
>     outputs?
>
> 3.  Under what circumstances would your new expression produce an error message
>     where the original one would not?
{: .challenge}

> ## Which Pipe?
>
> A file called animals.txt contains 586 lines of data formatted as follows:
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> ...
> ~~~
> {: .output}
>
> What command would you use to produce a table that shows
> the total count of each type of animal in the file?
>
> 1.  `grep {deer, rabbit, raccoon, deer, fox, bear} animals.txt | wc -l`
> 2.  `sort animals.txt | uniq -c`
> 3.  `sort -t, -k2,2 animals.txt | uniq -c`
> 4.  `cut -d, -f 2 animals.txt | uniq -c`
> 5.  `cut -d, -f 2 animals.txt | sort | uniq -c`
> 6.  `cut -d, -f 2 animals.txt | sort | uniq -c | wc -l`
{: .challenge}

> ## Appending Data
>
> Consider the file `animals.txt`, used in previous exercise.
> After these commands, select the alternative that
> corresponds the file `animalsUpd.txt`:
>
> ~~~
> $ head -3 animals.txt > animalsUpd.txt
> $ tail -2 animals.txt >> animalsUpd.txt
> ~~~
> {: .bash}
>
> 1. The first three lines of `animals.txt`
> 2. The last two lines of `animals.txt`
> 3. The first three lines and the last two lines of `animals.txt`
> 4. The second and third lines of `animals.txt`
{: .challenge}
