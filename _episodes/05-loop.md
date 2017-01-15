---
title: "Loops"
teaching: 15
exercises: 0
questions:
- "How can I perform the same actions on many different files?"
objectives:
- "Write a loop that applies one or more commands separately to each file in a set of files."
- "Trace the values taken on by a loop variable during execution of the loop."
- "Explain the difference between a variable's name and its value."
- "Explain why spaces and some punctuation characters shouldn't be used in file names."
- "Demonstrate how to see what commands have recently been executed."
- "Re-run recently executed commands without retyping them."
keypoints:
- "A `for` loop repeats commands once for every thing in a list."
- "Every `for` loop needs a variable to refer to the thing it is currently operating on."
- "Use `$name` to expand a variable (i.e., get its value). `${name}` can also be used."
- "Do not use spaces, quotes, or wildcard characters such as '*' or '?' in filenames, as it complicates variable expansion."
- "Give files consistent names that are easy to match with wildcard patterns to make it easy to select them for looping."
- "Use the up-arrow key to scroll up through previous commands to edit and repeat them."
- "Use `Ctrl-R` to search through the previously entered commands."
- "Use `history` to display recent commands, and `!number` to repeat a command by number."
---

** Loops ** son fundamentales para mejorar la productividad a través de la automatización, ya que nos permiten ejecutar
Comandos repetitivos. Al igual que los comodines y la terminación de fichas, el uso de bucles también
Cantidad de mecanografía (y errores de escritura).
Supongamos que tenemos varios cientos de archivos de datos del genoma denominados `basilisk.dat`,` unicorn.dat` y así sucesivamente.
En este ejemplo,
Usaremos el directorio `creatures` que sólo tiene dos archivos de ejemplo,
Pero los principios se pueden aplicar a muchos muchos más archivos a la vez.
Nos gustaría modificar estos archivos, pero también guardar una versión de los archivos originales, nombrar las copias
`Original-basilisk.dat` y` original-unicorn.dat`.
No podemos usar:

~~~
$ cp *.dat original-*.dat
~~~
{: .bash}

Porque eso se expandiría a:

~~~
$ cp basilisk.dat unicorn.dat original-*.dat
~~~
{: .bash}

Esto no respaldaría nuestros archivos, sino que obtendremos un error:

~~~
cp: target `original-*.dat` is not a directory
~~~
{: .error}

Este problema surge cuando `cp` recibe más de dos entradas. Cuando esto sucede,
Espera que la última entrada sea un directorio donde pueda copiar todos los archivos que se pasaron.
Dado que no existe ningún directorio denominado `original - *. Dat` en el directorio` creatures` obtendremos un
error.

En cambio, podemos usar un bucle ** **
Para hacer alguna operación una vez para cada cosa en una lista.
Aquí hay un ejemplo sencillo que muestra las tres primeras líneas de cada archivo a su vez:

~~~
$ for filename in basilisk.dat unicorn.dat
> do
>    head -n 3 $filename
> done
~~~
{: .bash}

~~~
COMMON NAME: basilisk
CLASSIFICATION: basiliscus vulgaris
UPDATED: 1745-05-02
COMMON NAME: unicorn
CLASSIFICATION: equus monoceros
UPDATED: 1738-11-24
~~~
{: .output}

Cuando el shell ve la palabra clave `for`,
Sabe repetir un comando (o grupo de comandos) una vez para cada cosa `in` en una lista.
Para cada iteración,
El nombre de cada cosa se asigna secuencialmente a
La variable ** ** y los comandos dentro del bucle se ejecutan antes de pasar a
La siguiente cosa en la lista.
Dentro del bucle,
Pedimos el valor de la variable poniendo `$` delante de ella.
El `$` le dice al intérprete de shell que trate
La variable ** ** como un nombre de variable y sustituir su valor en su lugar,
En lugar de tratarlo como texto o como un comando externo.

En este ejemplo, la lista tiene dos nombres de archivo: `basilisk.dat` y` unicorn.dat`.
Cada vez que el bucle se itera, asignará un nombre de archivo a la variable `filename`
Y ejecute el comando `head`.
La primera vez a través del bucle,
`$ Filename` es` basilisk.dat`.
El intérprete ejecuta el comando `head` en` basilisk.dat`,
Y las impresiones del
Primeras tres líneas de `basilisk.dat`.
Para la segunda iteración, `$ filename` se convierte en
`Unicorn.dat`. Esta vez, el shell ejecuta `head` en` unicorn.dat`
E imprime las tres primeras líneas de `unicorn.dat`.
Dado que la lista era sólo dos elementos, la shell sale del bucle `for`.

Cuando se utilizan variables, también es
Posible poner los nombres en llaves para delimitar claramente la variable
Nombre: `$ filename` es equivalente a` $ {filename} `, pero es diferente de
`$ {File} nombre`. Puede encontrar esta notación en los programas de otras personas.

> ## Siga el prompt
>
> El indicador de comandos cambia de `$` a `>` y de nuevo como estábamos
> Escribiendo en nuestro bucle. El segundo indicador, `>`, es diferente para recordar
> Nosotros que aún no hemos terminado de escribir un comando completo. Un punto y coma, `;`,
> Se puede utilizar para separar dos comandos escritos en una sola línea.
{: .callout}

> ## Los mismos símbolos, diferentes significados
>
> Aquí vemos `>` siendo utilizado un prompt de shell, mientras que `>` es también
> Utilizado para redirigir la salida.
> De forma similar, `$` se utiliza como indicador de shell, pero, como vimos antes,
> También se utiliza para pedir que el shell obtenga el valor de una variable.
>
> Si el * shell * imprime `>` o `$` entonces espera que escriba algo,
> Y el símbolo es un indicador.
>
> Si * usted * escribe `>` o `$` usted mismo, es una instrucción de usted que
> El shell para redirigir la salida o obtener el valor de una variable.
{: .callout}

Hemos llamado a la variable en este bucle `filename`
Con el fin de hacer su propósito más claro para los lectores humanos.
El shell en sí no le importa lo que la variable se llama;
Si escribimos este bucle como:

~~~
for x in basilisk.dat unicorn.dat
do
    head -n 3 $x
done
~~~
{: .bash}

or:

~~~
for temperature in basilisk.dat unicorn.dat
do
    head -n 3 $temperature
done
~~~
{: .bash}

Funcionaría exactamente de la misma manera.
* No lo hagas. *
Los programas sólo son útiles si la gente puede entenderlos,
Nombres tan insignificantes (como `x`) o nombres engañosos (como` temperature`)
Aumentar las probabilidades de que el programa no hará lo que sus lectores piensan que hace.

He aquí un bucle un poco más complicado:

~~~
for filename in *.dat
do
    echo $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

El shell comienza expandiendo `* .dat` para crear la lista de archivos que procesará.
El ** cuerpo de bucle **
Entonces ejecuta dos comandos para cada uno de esos archivos.
El primero, `echo`, simplemente imprime sus parámetros de línea de comandos a la salida estándar.
Por ejemplo:

~~~
$ echo hello there
~~~
{: .bash}

prints:

~~~
hello there
~~~
{: .output}

En este caso,
Ya que el shell se expande `$ filename` para que sea el nombre de un archivo,
`Echo $ filename` sólo imprime el nombre del archivo.
Tenga en cuenta que no podemos escribir esto como:

~~~
for filename in *.dat
do
    $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

Porque entonces la primera vez a través del bucle,
Cuando `$ filename` se expandió a `basilisk.dat`, el shell intentaría ejecutar `basilisk.dat` como un programa.
Finalmente,
La combinación `cabeza 'y' cola` selecciona las líneas 81-100
Desde cualquier archivo que se esté procesando
(Suponiendo que el archivo tenga al menos 100 líneas).

> ## Espacios en los nombres
>
> El espacio en blanco se utiliza para separar los elementos de la lista
> Que vamos a repetir. Si en la lista tenemos elementos
> Con espacios en blanco necesitamos citar esos elementos
> Y nuestra variable al usarlo.
> Supongamos que nuestros archivos de datos tienen el nombre:
>
> ~~~
> red dragon.dat
> purple unicorn.dat
> ~~~
> {: .source}
> 
> We need to use
> 
> ~~~
> for filename in "red dragon.dat" "purple unicorn.dat"
> do
>     head -n 100 "$filename" | tail -n 20
> done
> ~~~
> {: .bash}
>
> Es más sencillo evitar el uso de espacios en blanco (u otros caracteres especiales) en los nombres de archivo.
{: .callout}

Volviendo a nuestro problema original de copia de archivos,
Podemos resolverlo usando este bucle:

~~~
for filename in *.dat
do
    cp $filename original-$filename
done
~~~
{: .bash}

![For Loop in Action](../fig/shell_script_for_loop_flow_chart.svg)

Este bucle ejecuta el comando `cp` una vez para cada nombre de archivo.
La primera vez,
Cuando `$ filename` se expande a` basilisk.dat`,
El shell ejecuta:

~~~
cp basilisk.dat original-basilisk.dat
~~~
{: .bash}

The second time, the command is:

~~~
cp unicorn.dat original-unicorn.dat
~~~
{: .bash}

## Nelle's Pipeline: Processing Files

Nelle está ahora lista para procesar sus archivos de datos.
Desde que ella todavía está aprendiendo cómo utilizar la cáscara,
Ella decide construir los comandos requeridos en etapas.
Su primer paso es asegurarse de que ella puede seleccionar los archivos correctos --- recuerde,
Estos son aquellos cuyos nombres terminan en 'A' o 'B', en lugar de 'Z'. A partir de su directorio personal, los tipos Nelle:

~~~
$ cd north-pacific-gyre/2012-07-03
$ for datafile in *[AB].txt
> do
>     echo $datafile
> done
~~~
{: .bash}

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
~~~
{: .output}

Su siguiente paso es decidir
Cómo llamar a los archivos que creará el programa de análisis `goostats`.
Prefijar el nombre de cada archivo de entrada con "stats" parece simple,
Así que modifica su bucle para hacer eso:

~~~
$ for datafile in *[AB].txt
> do
>     echo $datafile stats-$datafile
> done
~~~
{: .bash}

~~~
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
~~~
{: .output}

Ella todavía no ha ejecutado `goostats`,
Pero ahora está segura de que puede seleccionar los archivos correctos y generar los nombres de archivo de salida correctos.

Escribir comandos una y otra vez es cada vez más tedioso,
aunque,
Y Nelle está preocupada por cometer errores,
Así que en lugar de volver a entrar en su bucle,
Ella presiona la flecha hacia arriba.
En respuesta,
El shell vuelve a mostrar el bucle completo en una línea
(Usando puntos y comas para separar las piezas):

~~~
$ for datafile in *[AB].txt; do echo $datafile stats-$datafile; done
~~~
{: .bash}

Utilizando la tecla de flecha izquierda,
Nelle realiza una copia de seguridad y cambia el comando `echo` a` bash goostats`:

~~~
$ for datafile in *[AB].txt; do bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

Cuando presione Enter,
El shell ejecuta el comando modificado.
Sin embargo, nada parece suceder --- no hay salida.
Después de un momento, Nelle se da cuenta de que ya que su guión ya no imprime nada a la pantalla,
Ella no tiene ni idea si está corriendo, y mucho menos qué tan rápido.
Mata el comando de ejecución escribiendo `Ctrl-C`,
Utiliza la flecha hacia arriba para repetir el comando,
Y lo edita para que lea:

~~~
$ for datafile in *[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

> ## Principio y Fin
>
> Podemos pasar al principio de una línea en el shell escribiendo `Ctrl-A`
> Y al final usando `Ctrl-E`.
{: .callout}

Cuando ella dirige su programa ahora,
Produce una línea de salida cada cinco segundos aproximadamente:

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~
{: .output}

1518 multiplicado por 5 segundos,
Dividido por 60,
Le dice que su script tomará alrededor de dos horas para correr.
Como un cheque final,
Abre otra ventana terminal,
Entra en `north-pacific-gyre / 2012-07-03`,
Y utiliza `cat stats-NENE01729B.txt`
Para examinar uno de los archivos de salida.
Se ve bien,
Así que decide tomar un café y ponerse al día con su lectura.

> ## Aquellos que saben que la historia puede elegir repetirlo
>
> Otra forma de repetir el trabajo anterior es usar el comando `history` para
> Obtener una lista de los últimos cientos de comandos que se han ejecutado, y
> Entonces para usar `! 123` (donde" 123 "es reemplazado por el número de comando) a
> Repetir uno de esos comandos. Por ejemplo, si Nelle escribe esto:
>
> ~~~
> $ history | tail -n 5
> ~~~
> {: .bash}
> ~~~
>   456  ls -l NENE0*.txt
>   457  rm stats-NENE01729B.txt.txt
>   458  bash goostats NENE01729B.txt stats-NENE01729B.txt
>   459  ls -l NENE0*.txt
>   460  history
> ~~~
> {: .output}
>
> Entonces puede volver a ejecutar `goostats` en` NENE01729B.txt` simplemente escribiendo
> `!458`.
{: .callout}

> ## Otros comandos de historial
>
> Existen varios otros comandos de acceso directo para acceder a la historia.
> Dos de los más útiles son `!!`, que recupera el
> Comando anterior (puede o no encontrar esto más conveniente que
> Plain up-arrow), y `! $`, Que recupera la última palabra de la última
> Comando. Es útil más a menudo de lo que cabría esperar: después de
> `Bash goostats NENE01729B.txt stats-NENE01729B.txt`, puede escribir
> `Less! $` Para ver el archivo `stats-NENE01729B.txt`, que es
> Más rápido que hacer la flecha hacia arriba y editar la línea de comandos.
{: .callout}

> ## Variables in Loops
>
> Suppose that `ls` initially displays:
>
> ~~~
> fructose.dat    glucose.dat   sucrose.dat
> ~~~
> {: .output}
>
> What is the output of:
>
> ~~~
> for datafile in *.dat
> do
>     ls *.dat
> done
> ~~~
> {: .bash}
>
> Now, what is the output of:
>
> ~~~
> for datafile in *.dat
> do
>	ls $datafile
> done
> ~~~
> {: .bash}
>
> Why do these two loops give different outputs?
{: .challenge}

> ## Saving to a File in a Loop - Part One
>
> In the same directory, what is the effect of this loop?
>
> ~~~
> for sugar in *.dat
> do
>     echo $sugar
>     cat $sugar > xylose.dat
> done
> ~~~
> {: .bash}
>
> 1.  Prints `fructose.dat`, `glucose.dat`, and `sucrose.dat`, and the text from `sucrose.dat` will be saved to a file called `xylose.dat`.
> 2.  Prints `fructose.dat`, `glucose.dat`, and `sucrose.dat`, and the text from all three files would be
>     concatenated and saved to a file called `xylose.dat`.
> 3.  Prints `fructose.dat`, `glucose.dat`, `sucrose.dat`, and
>     `xylose.dat`, and the text from `sucrose.dat` will be saved to a file called `xylose.dat`.
> 4.  None of the above.
{: .challenge}

> ## Saving to a File in a Loop - Part Two
>
> In another directory, where `ls` returns:
>
> ~~~
> fructose.dat    glucose.dat   sucrose.dat   maltose.txt
> ~~~
> {: .output}
>
> What would be the output of the following loop?
>
> ~~~
> for datafile in *.dat
> do
>     cat $datafile >> sugar.dat
> done
> ~~~
> {: .bash}
>
> 1.  All of the text from `fructose.dat`, `glucose.dat` and `sucrose.dat` would be
>     concatenated and saved to a file called `sugar.dat`.
> 2.  The text from `sucrose.dat` will be saved to a file called `sugar.dat`.
> 3.  All of the text from `fructose.dat`, `glucose.dat`, `sucrose.dat` and `maltose.txt`
>     would be concatenated and saved to a file called `sugar.dat`.
> 4.  All of the text from `fructose.dat`, `glucose.dat` and `sucrose.dat` would be printed
>     to the screen and saved to a file called `sugar.dat`
{: .challenge}

> ## Limiting Sets of Files
>
> In the same directory, where `ls` returns (without the `sugar.dat` file):
>
> ~~~
> fructose.dat    glucose.dat   sucrose.dat   maltose.txt
> ~~~
> {: .output}
> 
> What would be the output of the following loop?
>
> ~~~
> for filename in s*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1.  No files are listed.
> 2.  All files are listed.
> 3.  Only `fructose.dat`, `glucose.dat` and `maltose.txt` are listed.
> 4.  Only `sucrose.dat` is listed.
>
> How would the output differ from using this command instead?
>
> ~~~
> for filename in *s*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1.  The same files would be listed.
> 2.  All the files are listed this time.
> 3.  No files are listed this time.
> 4.  The file `sucrose.dat` will be listed twice, with the other files listed once each.
{: .challenge}

> ## Doing a Dry Run
>
> A loop is a way to do many things at once --- or to make many mistakes at
> once if it does the wrong thing. One way to check what a loop *would* do
> is to echo the commands it would run instead of actually running them.
> 
> Suppose we want to preview the commands the following loop will execute
> without actually running those commands:
>
> ~~~
> for file in *.dat
> do
>   analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> What is the difference between the two loops below, and which one would we
> want to run?
>
> ~~~
> # Version 1
> for file in *.dat
> do
>   echo analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> ~~~
> # Version 2
> for file in *.dat
> do
>   echo "analyze $file > analyzed-$file"
> done
> ~~~
> {: .bash}
{: .challenge}

> ## Nested Loops
>
> Suppose we want to set up up a directory structure to organize
> some experiments measuring the growth rate under different sugar
> types *and* different temperatures.  What would be the
> result of the following code:
>
> ~~~
> for sugar in fructose glucose sucrose
> do
>     for temperature in 25 30 37 40
>     do
>         mkdir $sugar-$temperature
>     done
> done
> ~~~
> {: .bash}
{: .challenge}
