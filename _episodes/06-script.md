---
title: "Shell Scripts"
teaching: 15
exercises: 0
questions:
- "How can I save and re-use commands?"
objectives:
- "Write a shell script that runs a command or series of commands for a fixed set of files."
- "Run a shell script from the command line."
- "Write a shell script that operates on a set of files defined by the user on the command line."
- "Create pipelines that include shell scripts you, and others, have written."
keypoints:
- "Save commands in files (usually called shell scripts) for re-use."
- "`bash filename` runs the commands saved in a file."
- "`$@` refers to all of a shell script's command-line parameters."
- "`$1`, `$2`, etc., refer to the first command-line parameter, the second command-line parameter, etc."
- "Place variables in quotes if the values might have spaces in them."
- "Letting users decide what files to process is more flexible and more consistent with built-in Unix commands."
---

Finalmente estamos listos para ver lo que hace que la shell sea un entorno de programación tan potente.
Vamos a tomar los comandos que repetimos con frecuencia y guardarlos en archivos
De modo que podemos volver a ejecutar todas esas operaciones de nuevo más tarde escribiendo un solo comando.
Por razones históricas,
Un montón de comandos guardados en un archivo se suele llamar un ** shell script **,
Pero no se equivoquen:
Estos son en realidad pequeños programas.

Comencemos por volver a `moléculas /` y poniendo la siguiente línea en un nuevo archivo, `middle.sh`:

~~~
$ cd molecules
$ nano middle.sh
~~~
{: .bash}

El comando `nano middle.sh` abre el archivo` middle.sh` dentro del editor de texto "nano"
(Que se ejecuta dentro de la concha).
Si el archivo no existe, se creará.
Podemos usar el editor de texto para editar directamente el archivo --- simplemente insertaremos la siguiente línea:

~~~
head -n 15 octane.pdb | tail -n 5
~~~
{: .source}

Esta es una variación de la tubería que construimos anteriormente:
Selecciona las líneas 11-15 del archivo `octane.pdb`.
Recuerde, no estamos * corriendo como un comando todavía:
Estamos poniendo los comandos en un archivo.

A continuación, guardamos el archivo (`Ctrl-O` en nano),
  Y salir del editor de texto (`Ctrl-X` en nano).
Compruebe que el directorio `moléculas` ahora contiene un archivo llamado` middle.sh`.

Una vez que hayamos guardado el archivo,
Podemos pedirle al shell que ejecute los comandos que contiene.
Nuestro shell se llama `bash`, por lo que ejecutamos el siguiente comando:

~~~
$ bash middle.sh
~~~
{: .bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

Bastante seguro,
La salida de nuestro guión es exactamente lo que obtendríamos si ejecutamos esa tubería directamente.

> ## Texto vs. Lo que sea
>
> Por lo general llamamos a programas como Microsoft Word o LibreOffice Writer "text
> Editores ", pero tenemos que ser un poco más cuidadosos cuando se trata de
> Programación. De forma predeterminada, Microsoft Word utiliza archivos .docx para almacenar no
> Sólo texto, sino también formatear información sobre fuentes, encabezados y
> Encendido. Esta información adicional no se almacena como caracteres y no significa
> Nada a herramientas como `head`: esperan que los archivos de entrada contengan
> Nada más que las letras, los dígitos y la puntuación en un equipo estándar
> Teclado. Por lo tanto, al editar programas, debe utilizar un
> Editor de texto, o tenga cuidado de guardar archivos como texto sin formato.
{: .callout}

¿Qué pasa si queremos seleccionar líneas de un archivo arbitrario?
Podríamos editar `middle.sh` cada vez para cambiar el nombre de archivo,
Pero que probablemente llevaría más tiempo que simplemente volver a escribir el comando.
En su lugar, vamos a editar `middle.sh` y hacerlo más versátil:

~~~
$ nano middle.sh
~~~
{: .bash}

Ahora, dentro de "nano", reemplace el texto `octane.pdb` con la variable especial llamada` $ 1`:

~~~
head -n 15 "$1" | tail -n 5
~~~
{: .output}

Dentro de un script de shell,
`$ 1` significa" el primer nombre de archivo (u otro parámetro) en la línea de comandos ".
Ahora podemos ejecutar nuestro script como este:

~~~
$ bash middle.sh octane.pdb
~~~
{: .bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

or on a different file like this:

~~~
$ bash middle.sh pentane.pdb
~~~
{: .bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

> ## Double-Quotes Around Arguments
>
> Por la misma razón que ponemos la variable loop dentro de comillas dobles,
> En caso de que el nombre de archivo contenga espacios,
> Rodeamos `$ 1` con comillas dobles.
{: .callout}

Aún necesitamos editar `middle.sh` cada vez que queramos ajustar el rango de líneas,
aunque.
Vamos a arreglar eso usando las variables especiales `$ 2` y` $ 3` para el
Número de líneas que se pasarán respectivamente a `head` y` tail`:

~~~
$ nano middle.sh
~~~
{: .bash}

~~~
head -n "$2" "$1" | tail -n "$3"
~~~
{: .output}

We can now run:

~~~
$ bash middle.sh pentane.pdb 15 5
~~~
{: .bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

Cambiando los argumentos a nuestro comando, podemos cambiar el script
comportamiento:

~~~
$ bash middle.sh pentane.pdb 20 5
~~~
{: .bash}

~~~
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
~~~
{: .output}

Esto funciona,
Pero puede tomar a la siguiente persona que lee `middle.sh` un momento para averiguar lo que hace.
Podemos mejorar nuestro script agregando algunos ** comentarios ** en la parte superior:

~~~
$ nano middle.sh
~~~
{: .bash}

~~~
# Select lines from the middle of a file.
# Usage: bash middle.sh filename end_line num_lines
head -n "$2" "$1" | tail -n "$3"
~~~
{: .output}

Un comentario comienza con un caracter `#` y se ejecuta hasta el final de la línea.
La computadora ignora los comentarios,
Pero son invaluables para ayudar a la gente (incluyendo a tu futuro) a entender y usar scripts.
La única advertencia es que cada vez que modifique el script,
Debe comprobar que el comentario sigue siendo exacto:
Una explicación que envía al lector en la dirección equivocada es peor que ninguna en absoluto.

¿Qué sucede si queremos procesar muchos archivos en un único oleoducto?
Por ejemplo, si queremos ordenar nuestros archivos `.pdb` por longitud, deberíamos escribir:

~~~
$ wc -l *.pdb | sort -n
~~~
{: .bash}

Porque `wc -l` lista el número de líneas en los archivos
(Recuerde que `wc` significa 'word count', agregando el` -l` significa 'count lines' en su lugar)
Y `sort -n` ordena las cosas numéricamente.
Podríamos poner esto en un archivo,
Pero entonces sólo podría ordenar una lista de archivos `.pdb` en el directorio actual.
Si queremos ser capaces de obtener una lista ordenada de otros tipos de archivos,
Necesitamos una manera de conseguir todos esos nombres en el guión.
No podemos usar `$ 1`,` $2`, y así sucesivamente
Porque no sabemos cuántos archivos hay.
En su lugar, utilizamos la variable especial `$ @`,
lo que significa,
"Todos los parámetros de la línea de comandos a la secuencia de comandos de shell."
También debemos poner `$ @` dentro de comillas dobles
Para manejar el caso de los parámetros que contienen espacios
(`"$@"` Es equivalente a `"$1"` `"$2"`...)
He aquí un ejemplo:

~~~
$ nano sorted.sh
~~~
{: .bash}

~~~
# Sort filenames by their length.
# Usage: bash sorted.sh one_or_more_filenames
wc -l "$@" | sort -n
~~~
{: .output}

~~~
$ bash sorted.sh *.pdb ../creatures/*.dat
~~~
{: .bash}

~~~
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/unicorn.dat
~~~
{: .output}

> ## ¿Por qué no está haciendo nada?
>
> ¿Qué sucede si se supone que un script procesa un montón de archivos, pero nosotros
> No le dan nombres de archivo? Por ejemplo, ¿qué pasa si escribimos:
>
> ~~~
> $ bash sorted.sh
> ~~~
> {: .bash}
>
> Pero no diga `* .dat` (o cualquier otra cosa)? En este caso, `$ @` se expande a
> Nada en absoluto, por lo que el pipe dentro del script es efectivamente:
>
> ~~~
> $ wc -l | sort -n
> ~~~
> {: .bash}
>
> Dado que no tiene ningún nombre de archivo, `wc` supone que se supone que
> Proceso de entrada estándar, por lo que sólo se sienta allí y espera a que nos dé
> Algunos datos interactivamente. Desde el exterior, sin embargo, todo lo que vemos es
> Sentado allí: el script no parece hacer nada.
{: .callout}

Suponga que acabamos de ejecutar una serie de comandos que hicieron algo útil --- por ejemplo,
Que creó un gráfico que nos gustaría utilizar en un documento.
Nos gustaría poder volver a crear el gráfico más tarde si es necesario,
Por lo que queremos guardar los comandos en un archivo.
En lugar de escribirlas de nuevo
(Y potencialmente equivocarse)
Podemos hacer esto:

~~~
$ history | tail -n 5 > redo-figure-3.sh
~~~
{: .bash}

The file `redo-figure-3.sh` now contains:

~~~
297 bash goostats -r NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
~~~
{: .source}

Después de un momento de trabajo en un editor para eliminar los números de serie en los comandos,
Y para eliminar la línea final donde llamamos el comando `history`,
Tenemos un registro completamente exacto de cómo creamos esa figura.

En la práctica, la mayoría de las personas desarrollan scripts de shell ejecutando comandos en el indicador de shell varias veces
Para asegurarse de que están haciendo lo correcto,
A continuación, guardarlos en un archivo para su reutilización.
Este estilo de trabajo permite a la gente reciclar
Qué descubren sobre sus datos y su flujo de trabajo con una llamada a `history`
Y un poco de edición para limpiar la salida
Y guárdelo como un script de shell.

## Pipeline de Nelle: Creación de una secuencia de comandos

Un comentario extraño de su supervisor ha hecho que Nelle se dé cuenta de que
Ella debería haber proporcionado un par de parámetros adicionales a `goostats` cuando procesó sus archivos.
Esto podría haber sido un desastre si hubiera hecho todo el análisis a mano,
Pero gracias a los bucles `for`,
Sólo tomará un par de horas para volver a hacer.

Pero la experiencia le ha enseñado que si algo tiene que hacerse dos veces,
Probablemente tendrá que hacerse una tercera o cuarta vez también.
Ella dirige el editor y escribe lo siguiente:

~~~
# Calculate reduced stats for data files at J = 100 c/bp.
for datafile in "$@"
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~
{: .bash}

(Los parámetros `-J 100` y` -r` son los que su supervisor dijo que debería haber usado.)
Guarda esto en un archivo llamado `do-stats.sh`
Para que ahora pueda volver a hacer la primera etapa de su análisis escribiendo:

~~~
$ bash do-stats.sh *[AB].txt
~~~
{: .bash}

She can also do this:

~~~
$ bash do-stats.sh *[AB].txt | wc -l
~~~
{: .bash}

De modo que la salida es sólo el número de archivos procesados
En lugar de los nombres de los archivos que se procesaron.

Una cosa a tener en cuenta sobre el guión de Nelle es que
Permite a la persona que lo ejecuta decidir qué archivos procesar.
Podría haberlo escrito así:

~~~
# Calculate reduced stats for  A and Site B data files at J = 100 c/bp.
for datafile in *[AB].txt
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~
{: .bash}

La ventaja es que siempre selecciona los archivos correctos:
Ella no tiene que recordar excluir los archivos 'Z'.
La desventaja es que * siempre * selecciona sólo esos archivos --- ella no puede ejecutarlo en todos los archivos
(Incluidos los ficheros "Z"),
O en los archivos 'G' o 'H' que sus colegas de la Antártida están produciendo,
Sin editar el guión.
Si quería ser más aventurera,
Ella podría modificar su script para comprobar los parámetros de la línea de comandos,
Y utilice `* [AB] .txt` si no se ha proporcionado ninguno.
Por supuesto, esto introduce otro equilibrio entre flexibilidad y complejidad.

> ## Variables in Shell Scripts
>
> In the `molecules` directory, imagine you have a shell script called `script.sh` containing the
> following commands:
>
> ~~~
> head -n $2 $1
> tail -n $3 $1
> ~~~
> {: .bash}
>
> While you are in the `molecules` directory, you type the following command:
>
> ~~~
> bash script.sh '*.pdb' 1 1
> ~~~
> {: .bash}
>
> Which of the following outputs would you expect to see?
>
> 1. All of the lines between the first and the last lines of each file ending in `.pdb`
>    in the `molecules` directory
> 2. The first and the last line of each file ending in `.pdb` in the `molecules` directory
> 3. The first and the last line of each file in the `molecules` directory
> 4. An error because of the quotes around `*.pdb`
{: .challenge}

> ## List Unique Species
>
> Leah has several hundred data files, each of which is formatted like this:
>
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> 2013-11-06,fox,1
> 2013-11-07,rabbit,18
> 2013-11-07,bear,1
> ~~~
> {: .source}
>
> Write a shell script called `species.sh` that takes any number of
> filenames as command-line parameters, and uses `cut`, `sort`, and
> `uniq` to print a list of the unique species appearing in each of
> those files separately.
{: .challenge}

> ## Find the Longest File With a Given Extension
>
> Write a shell script called `longest.sh` that takes the name of a
> directory and a filename extension as its parameters, and prints
> out the name of the file with the most lines in that directory
> with that extension. For example:
>
> ~~~
> $ bash longest.sh /tmp/data pdb
> ~~~
> {: .bash}
>
> would print the name of the `.pdb` file in `/tmp/data` that has
> the most lines.
{: .challenge}

> ## Why Record Commands in the History Before Running Them?
>
> If you run the command:
>
> ~~~
> $ history | tail -n 5 > recent.sh
> ~~~
> {: .bash}
>
> the last command in the file is the `history` command itself, i.e.,
> the shell has added `history` to the command log before actually
> running it. In fact, the shell *always* adds commands to the log
> before running them. Why do you think it does this?
{: .challenge}

> ## Script Reading Comprehension
>
> Joel's `data` directory contains three files: `fructose.dat`,
> `glucose.dat`, and `sucrose.dat`. Explain what a script called
> `example.sh` would do when run as `bash example.sh *.dat` if it
> contained the following lines:
>
> ~~~
> # Script 1
> echo *.*
> ~~~
> {: .bash}
>
> ~~~
> # Script 2
> for filename in $1 $2 $3
> do
>     cat $filename
> done
> ~~~
> {: .bash}
>
> ~~~
> # Script 3
> echo $@.dat
> ~~~
> {: .bash}
{: .challenge}

> ## Debugging Scripts
>
> Suppose you have saved the following script in a file called `do-errors.sh`
> in Nelle's `north-pacific-gyre/2012-07-03` directory:
>
> ~~~
> # Calculate reduced stats for data files at J = 100 c/bp.
> for datafile in "$@"
> do
>     echo $datfile
>     bash goostats -J 100 -r $datafile stats-$datafile
> done
> ~~~
> {: .bash}
>
> When you run it:
>
> ~~~
> $ bash do-errors.sh *[AB].txt
> ~~~
> {: .bash}
>
> the output is blank.
> To figure out why, re-run the script using the `-x` option:
>
> ~~~
> bash -x do-errors.sh *[AB].txt
> ~~~
> {: .bash}
>
> What is the output showing you?
> Which line is responsible for the error?
{: .challenge}
