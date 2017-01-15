---
title: "Navigating Files and Directories"
teaching: 15
exercises: 0
questions:
- "How can I move around on my computer?"
- "How can I see what files and directories I have?"
- "How can I specify the location of a file or directory on my computer?"
objectives:
- "Explain the similarities and differences between a file and a directory."
- "Translate an absolute path into a relative path and vice versa."
- "Construct absolute and relative paths that identify specific files and directories."
- "Explain the steps in the shell's read-run-print cycle."
- "Identify the actual command, flags, and filenames in a command-line call."
- "Demonstrate the use of tab completion, and explain its advantages."
keypoints:
- "The file system is responsible for managing information on the disk."
- "Information is stored in files, which are stored in directories (folders)."
- "Directories can also store other directories, which forms a directory tree."
- "`cd path` changes the current working directory."
- "`ls path` prints a listing of a specific file or directory; `ls` on its own lists the current working directory."
- "`pwd` prints the user's current working directory."
- "`whoami` shows the user's current identity."
- "`/` on its own is the root directory of the whole file system."
- "A relative path specifies a location starting from the current location."
- "An absolute path specifies a location from the root of the file system."
- "Directory names in a path are separated with '/' on Unix, but '\\\\' on Windows."
- "'..' means 'the directory above the current one'; '.' on its own means 'the current directory'."
- "Most files' names are `something.extension`. The extension isn't required, and doesn't guarantee anything, but is normally used to indicate the type of data in the file."
- "Most commands take options (flags) which begin with a '-'."
---

La parte del sistema operativo responsable de administrar archivos y directorios
Se denomina ** sistema de archivos **.
Organiza nuestros datos en archivos,
Que contienen información,
Y directorios (también llamados "carpetas"),
Que contienen archivos u otros directorios.

Varios comandos se utilizan con frecuencia para crear, inspeccionar, cambiar el nombre y eliminar archivos y directorios.
Para comenzar a explorarlos,
Vamos a abrir una ventana de shell:

> ## Preparación Magic
>
> Si escribe el comando:
> `PS1 = '$'`
> En su shell, seguido de presionar la tecla 'enter',
> Su ventana debe verse como nuestro ejemplo en esta lección.
> Esto no es necesario seguir (de hecho, su mensaje puede tener
> Otra información útil sobre la que desea saber). ¡Esto depende de ti! 
{: .callout}

~~~
$
~~~
{: .bash}

El signo de dólar es un ** indicador **, que nos demuestra que la cáscara está esperando la entrada;
Su shell puede usar un carácter diferente como un prompt y puede agregar información antes
El indicador. Al escribir comandos, ya sea a partir de estas lecciones o de otras fuentes,
No escriba el mensaje, sólo los comandos que lo sigan.

Escriba el comando `whoami`,
Luego presione la tecla Enter (a veces marcada Return) para enviar el comando al shell.
La salida del comando es la ID del usuario actual,
es decir.,
Nos muestra quién es el casco que creemos que somos:

~~~
$ whoami
~~~
{: .bash}

~~~
nelle
~~~
{: .output}

Más específicamente, cuando escribimos `whoami` el shell:

1. encuentra un programa llamado `whoami`,
2. ejecuta ese programa,
3. muestra la salida del programa, luego
4. muestra un nuevo mensaje para decirnos que está listo para más comandos.


> ## Variación de nombre de usuario
>
> En esta lección, hemos utilizado el nombre de usuario `nelle` (asociado
> Con nuestro científico hipotético Nelle) en el ejemplo de entrada y salida en todo.
> Sin embargo, cuando
> Escribe los comandos de esta lección en tu computadora,
> Deberías ver y usar algo diferente,
> A saber, el nombre de usuario asociado con la cuenta de usuario en su computadora. Esta
> Username será la salida de `whoami`. En
> Lo que sigue, `nelle` siempre debe ser reemplazado por ese nombre de usuario.
{: .callout}

Siguiente,
Vamos a averiguar dónde estamos ejecutando un comando llamado `pwd`
(Que significa "print working directory").
En cualquier momento,
Nuestro ** directorio de trabajo actual **
Es nuestro directorio predeterminado actual,
es decir.,
El directorio que la computadora asume que queremos ejecutar comandos en
A menos que especifiquemos explícitamente otra cosa.
Aquí,
La respuesta de la computadora es `/ Users / nelle`,
Que es Nelle's ** directorio personal **:
~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle
~~~
{: .output}

> ## Variación del Directorio Personal
>
> La ruta del directorio de inicio se verá diferente en diferentes sistemas operativos.
> En Linux puede parecer `/ home / nelle`,
> Y en Windows será similar a `C: \ Documents and Settings \ nelle` o
> `C: \ Users \ nelle`.
> (Tenga en cuenta que puede ser un poco diferente para diferentes versiones de Windows.)
> En futuros ejemplos, hemos utilizado la salida Mac como predeterminada: Linux y Windows
> La salida puede diferir ligeramente, pero debe ser generalmente similar.
{: .gritar}

Para entender lo que es un "directorio de inicio"
Echemos un vistazo a cómo se organiza el sistema de archivos como un todo. Para el
Por ejemplo, seremos
Ilustrando el sistema de archivos en la computadora de nuestro científico Nelle. Después de este
Ilustración, aprenderás comandos para explorar tu propio sistema de archivos,
Que se construirá de una manera similar, pero no será exactamente idéntica. 

En el ordenador de Nelle, el sistema de ficheros se ve así:

! [El sistema de archivos] (../ fig / filesystem.svg)

En la parte superior se encuentra el ** directorio raíz **
Que tiene todo lo demás.
Nos referimos a ella usando un caracter de barra `/` por su cuenta;
Esta es la barra principal en `/ Users / nelle`.

Dentro de ese directorio hay varios otros directorios:
`Bin` (que es donde se almacenan algunos programas integrados),
`Data` (para archivos de datos diversos),
`Usuarios` (donde se encuentran los directorios personales de los usuarios),
`Tmp` (para archivos temporales que no necesitan ser almacenados a largo plazo),
y así.

Sabemos que nuestro actual directorio de trabajo `/ Users / nelle` se almacena dentro de` / Users`
Porque `/ Users` es la primera parte de su nombre.
Similar,
Sabemos que `/ Users` se almacena dentro del directorio raíz` / `
Porque su nombre comienza con `/`.

> ## Slashes
>
> Observe que hay dos significados para el carácter `/`.
> Cuando aparezca en la parte frontal de un nombre de archivo o directorio,
> Se refiere al directorio raíz. Cuando aparece * dentro de * un nombre,
> Es sólo un separador.
{: .callout}

Debajo de `/ Users`,
Encontramos un directorio para cada usuario con una cuenta en la máquina de Nelle,
Sus colegas la momia y el hombre lobo.

! [Directorios de inicio] (../ fig / home-directories.svg)

Los archivos de la momia se almacenan en `/ Users / imhotep`,
Wolfman está en `/ Users / larry`,
Y Nelle's en `/ Users / nelle`. Porque Nelle es el usuario en nuestro
Ejemplos aquí, es por eso que recibimos `/ Users / nelle` como nuestro directorio personal.
Normalmente, cuando abre un nuevo símbolo del sistema, estará en
Su directorio de inicio para comenzar.

Ahora vamos a aprender el comando que nos permitirá ver el contenido de nuestro
Propio sistema de archivos. Podemos ver lo que hay en nuestro directorio personal ejecutando `ls`,
Que significa "listado":

~~~
$ ls
~~~
{: .bash}

~~~
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

(Una vez más, sus resultados pueden ser ligeramente diferentes dependiendo de su funcionamiento
Y cómo ha personalizado su sistema de archivos.)

`Ls` imprime los nombres de los archivos y directorios en el directorio actual en
orden alfabetico,
Dispuestos cuidadosamente en columnas.
Podemos hacer su salida más comprensible usando el ** flag ** `-F`,
Que le dice `ls` para agregar un` / `a los nombres de los directorios:

~~~
$ ls -F
~~~
{: .bash}

~~~
Applications/ Documents/    Library/      Music/        Public/
Desktop/      Downloads/    Movies/       Pictures/
~~~
{: .output}

`ls` tiene muchas otras opciones. 
~~~
{: .output}

Muchos comandos bash, y programas que la gente ha escrito que pueden ser
Ejecutar desde dentro de bash, apoyar un indicador `--help` para mostrar más
Información sobre cómo usar los comandos o programas.

Para más información sobre cómo usar `ls` podemos escribir` man ls`.
`Man` es el comando" manual "de Unix:
Imprime una descripción de un comando y sus opciones,
Y (si tienes suerte) proporciona algunos ejemplos de cómo usarlo.

> ## `man` y Git para Windows
>
> La shell de bash proporcionada por Git para Windows no
> Apoyar el comando `man`. Hacer una búsqueda en la web para
> `UNIX man page COMMAND` (por ejemplo,` unix man page grep`)
> Proporciona enlaces a numerosas copias del manual de Unix
> Páginas en línea.
> Por ejemplo, GNU proporciona enlaces a su
> [Manuales] (http://www.gnu.org/manual/manual.html):
> Estos incluyen [grep] (http://www.gnu.org/software/grep/manual/),
> Y el
> [Utilidades básicas de GNU] (http://www.gnu.org/software/coreutils/manual/coreutils.html),
> Que cubre muchos comandos introducidos en esta lección.
{: .callout}

Para navegar por las páginas `man`,
Puede usar las teclas de flecha arriba y abajo para mover línea por línea,
O pruebe las teclas "b" y la barra espaciadora para saltar hacia arriba y hacia abajo por página completa.
Salga de las páginas `man` escribiendo" q ".

Aquí,
Podemos ver que nuestro directorio home contiene principalmente ** sub-directorios **.
Cualquier nombre en su salida que no tenga barras,
Son viejos ** archivos **.
Y observe que hay un espacio entre `ls` y` -F`:
sin ello,
El shell cree que estamos tratando de ejecutar un comando llamado `ls-F`,
Que no existe.

> ## Parámetros vs. Argumentos
>
> De acuerdo con [Wikipedia] (https://en.wikipedia.org/wiki/Parameter_ (programa_de_computador) #Parameters_and_arguments),
> Los términos ** argumento ** y ** parámetro **
> Significa cosas ligeramente diferentes.
> En la práctica,
> Sin embargo,
> La mayoría de la gente los usa indistintamente o inconsistentemente,
> Así que nosotros también.
{: .callout}

También podemos usar `ls` para ver el contenido de un directorio diferente. Tomemos un
Mira nuestro directorio `Desktop` ejecutando` ls -F Desktop`,
es decir.,
El comando `ls` con los ** argumentos **` -F` y `Desktop`.
El segundo argumento --- el * sin * un guión principal --- dice `ls` que
Queremos un listado de algo que no sea nuestro directorio de trabajo actual:

~~~
$ ls -F Desktop
~~~
{: .bash}

~~~
data-shell/
~~~
{: .output}

Su salida debe ser una lista de todos los archivos y subdirectorios de su
Desktop, incluido el directorio `data-shell` que ha descargado en
El comienzo de la lección. Echa un vistazo a tu escritorio para confirmar que
Su salida es exacta.

Como puede ver ahora, el uso de un shell bash depende fuertemente de la idea de que
Sus archivos se organizan en un sistema de archivos jerárquico.
Organizar las cosas jerárquicamente de esta manera nos ayuda a realizar un seguimiento de nuestro trabajo:
Es posible poner centenares de archivos en nuestro directorio casero,
Así como es posible acumular cientos de papeles impresos en nuestro escritorio,
Pero es una estrategia autodestructiva.

Ahora que sabemos que el directorio `data-shell` se encuentra en nuestro escritorio,
Puede hacer dos cosas.

Primero, podemos ver su contenido, usando la misma estrategia que antes, pasando
Un nombre de directorio a `ls`:
~~~
$ ls -F Desktop/data-shell
~~~
{: .bash}

~~~
creatures/          molecules/          notes.txt           solar.pdf
data/               north-pacific-gyre/ pizza.cfg           writing/
~~~
{: .output}

En segundo lugar, podemos cambiar nuestra ubicación a un directorio diferente, por lo que
Ya no estamos ubicados en
Nuestro directorio personal.

El comando para cambiar las ubicaciones es `cd` seguido de un
Nombre de directorio para cambiar nuestro directorio de trabajo.
`Cd` significa" directorio de cambios ",
Que es un poco engañoso:
El comando no cambia el directorio,
Cambia la idea del shell de qué directorio estamos.

Digamos que queremos pasar al directorio `data` que vimos anteriormente. Podemos
Utilice la siguiente serie de comandos para llegar allí:

~~~
$ cd Desktop
$ cd data-shell
$ cd data
~~~
{: .bash}

Estos comandos nos moverán de nuestro directorio de inicio a nuestro Escritorio, luego a
El directorio `data-shell`, y luego en el directorio` data`. `Cd` no imprime nada,
Pero si ejecutamos 'pwd' después de esto, podemos ver que ahora estamos
En `/ Users / nelle / Desktop / data-shell / data`.
Si ejecutamos `ls` sin argumentos ahora,
Lista los contenidos de `/ Users / nelle / Desktop / data-shell / data`,
Porque ahí es donde estamos ahora:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell/data
~~~
{: .output}

~~~
$ ls -F
~~~
{: .bash}

~~~
amino-acids.txt   elements/     pdb/	        salmon.txt
animals.txt       morse.txt     planets.txt     sunspot.txt
~~~
{: .output}

Ahora sabemos cómo ir por el árbol de directorios, pero
Como subimos Podemos probar lo siguiente:

~~~
cd data-shell
~~~
{: .bash}

~~~
-bash: cd: data-shell: No such file or directory
~~~
{: .error}

¡Pero tenemos un error! ¿Por qué es esto?

Con nuestros métodos hasta ahora,
`Cd` sólo puede ver subdirectorios dentro de su directorio actual. Existen
Diferentes maneras de ver los directorios por encima de su ubicación actual; Empezaremos
Con el más simple.

Hay un acceso directo en el shell para subir un nivel de directorio
Que se parece a esto:

~~~
$ cd ..
~~~
{: .bash}

`..` es un nombre de directorio especial significado
"El directorio que contiene este",
O más sucintamente,
El ** padre ** del directorio actual.
Bastante seguro,
Si ejecutamos `pwd` después de ejecutar` cd ..`, volvemos a `/ Users / nelle / Desktop / data-shell`:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

Normalmente no aparece el directorio especial `..` cuando ejecutamos` ls`. Si queremos
Para mostrarlo, podemos dar `ls` el indicador` -a`:

~~~
$ ls -F -a
~~~
{: .bash}

~~~
./                  creatures/          notes.txt
../                 data/               pizza.cfg
.bash_profile       molecules/          solar.pdf
Desktop/            north-pacific-gyre/ writing/
~~~
{: .output}

`-a` significa" mostrar todo ";
Obliga a `ls` a mostrarnos nombres de archivos y directorios que comienzan con` .`,
Como `..` (que, si estamos en` / Users / nelle`, hace referencia al directorio `/ Users`)
Como puedes ver,
También muestra otro directorio especial que se llama simplemente `.`,
Que significa "el directorio de trabajo actual".
Puede parecer redundante tener un nombre para él,
Pero veremos algunos usos para ello pronto.

Tenga en cuenta que en la mayoría de las herramientas de línea de comandos, se pueden combinar varios parámetros
Con un solo `-` y sin espacios entre los parámetros:` ls -F -a` es
Equivalente a `ls -Fa`.

> Otros archivos ocultos
>
> Además de los directorios ocultos `..` y` .`, también puede ver un archivo
> Llamado `.bash_profile`. Este archivo normalmente contiene configuración de shell
> Ajustes. También puede ver otros archivos y directorios que comienzan
> Con `.`. Estos son generalmente archivos y directorios que se utilizan para configurar
> Programas diferentes en su computadora. El prefijo `.` se utiliza para evitar que
> Archivos de configuración de llenar el terminal cuando un comando `ls` estándar
> Se utiliza.
{: .callout}

> ## Ortogonalidad
>
> Los nombres especiales `.` y` ..` no pertenecen a `cd`;
> Son interpretados de la misma manera por cada programa.
> Por ejemplo,
> Si estamos en `/ Users / nelle / data`,
> El comando `ls ..` nos dará una lista de` / Users / nelle`.
> Cuando los significados de las partes son los mismos no importa cómo se combinan,
> Programadores dicen que son ** ortogonales **:
> Los sistemas ortogonales tienden a ser más fáciles de aprender
> Porque hay menos casos especiales y excepciones a seguir.
{: .callout}

Estos son los comandos básicos para navegar por el sistema de archivos en su computadora:
`Pwd`,` ls` y `cd`. Vamos a explorar algunas variaciones en esos comandos. Que pasa
si escribe `cd` por sí solo, sin dar un directorio? 

~~~
$ cd
~~~
{: .bash}

¿Cómo puede comprobar lo que sucedió? `Pwd` nos da la respuesta!

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle
~~~
{: .output}

Resulta que `cd` sin un argumento te devolverá a tu directorio de inicio,
Que es genial si te has perdido en tu propio sistema de archivos.

Vamos a intentar volver al directorio `data` desde antes. La última vez, usamos
Tres comandos, pero en realidad podemos enlazar la lista de directorios
Para pasar a `data` en un solo paso:

~~~
$ cd Desktop/data-shell/data
~~~
{: .bash}
Compruebe que nos hemos desplazado al lugar correcto ejecutando `pwd` y` ls -F`

Si queremos subir un nivel desde el directorio de datos, podríamos usar `cd ..`. Pero
Hay otra forma de moverse a cualquier directorio, independientemente de su
ubicación actual.

Hasta ahora, al especificar nombres de directorio, o incluso una ruta de directorio (como anteriormente),
Hemos estado usando ** caminos relativos **. Cuando utiliza una ruta relativa con un comando
Como `ls` o` cd`, intenta encontrar esa ubicación desde donde estamos,
En lugar de la raíz del sistema de archivos.

Sin embargo, es posible especificar la ** ruta absoluta ** a un directorio por
Incluyendo su ruta completa desde el directorio raíz, que está indicado por un
Principal barra. El `/` principal le dice a la computadora que siga el camino desde
La raíz del sistema de archivos, por lo que siempre se refiere a exactamente un directorio,
No importa donde estamos cuando ejecutamos el comando.

Esto nos permite pasar a nuestro directorio `data-shell` desde cualquier lugar
El sistema de ficheros (incluyendo desde `datos`). Para encontrar el camino absoluto
Estamos buscando, podemos usar `pwd` y luego extraer la pieza que necesitamos
Para pasar a `data-shell`.

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell/data
~~~
{: .output}

~~~
$ cd /Users/nelle/Desktop/data-shell
~~~
{: .bash}

Ejecute `pwd` y` ls -F` para asegurarse de que estamos en el directorio que esperamos.

> ## Dos más accesos directos
>
> La shell interpreta el carácter `~` (tilde) al inicio de una ruta a
> Significa "el directorio inicial del usuario actual". Por ejemplo, si el hogar de Nelle
> Es `/ Users / nelle`, entonces` ~ / data` es equivalente a
> `/ Users / nelle / data`. Esto sólo funciona si es el primer carácter en la
> Path: `here / there / ~ / elsewhere` es * not *` aquí / allí / Users / nelle / elsewhere`.
>
> Otro atajo es el carácter `-` (guión). `Cd` traducirá
> * El directorio anterior que estaba en *, que es más rápido que tener que recordar,
> Luego escriba, la ruta completa. Esta es una manera * muy * eficiente de retroceder
> Y hacia adelante entre directorios. La diferencia entre `cd ..` y` cd -` es
> Que el primero te trae * up *, mientras que el último te trae * back *. Usted puede
> Piensa en ello como el botón * Last Channel * en un control remoto de TV.
{: .callout}

### Pipeline de Nelle: Organización de archivos

Sabiendo esto de archivos y directorios,
Nelle está lista para organizar los archivos que creará la máquina de análisis de proteínas.
Primero,
Crea un directorio llamado `north-pacific-gyre`
(Para recordar de dónde provienen los datos).
Dentro de eso,
Crea un directorio llamado `2012-07-03`,
Que es la fecha en que comenzó a procesar las muestras.
Solía ​​utilizar nombres como «conference-paper» y «revised-results»,
Pero los encontró difíciles de entender después de un par de años.
(La última gota fue cuando se encontró creando
Un directorio denominado `revised-revised-results-3`.)

> ## Clasificación de salida
>
> Nelle nombra sus directorios "año-mes-día",
> Con ceros a la cabeza durante meses y días,
> Porque el shell muestra los nombres de archivos y directorios en orden alfabético.
> Si usaba nombres de mes,
Diciembre vendría antes de julio;
> Si no utiliza ceros a la izquierda,
> Noviembre ('11') vendría antes de julio ('7'). Del mismo modo, poner el año primero
> Significa que junio de 2012 llegará antes de junio de 2013.
{: .callout}

Cada una de sus muestras físicas está etiquetada según la convención de su laboratorio
Con una identificación única de diez caracteres,
Tal como "NENE01729A".
Esto es lo que utilizó en su registro de la colección
Para registrar la ubicación, el tiempo, la profundidad y otras características de la muestra,
Por lo que decide utilizarlo como parte del nombre de cada archivo de datos.
Dado que la salida de la máquina de ensayo es texto sin formato,
Ella llamará a sus archivos `NENE01729A.txt`,` NENE01812A.txt`, y así sucesivamente.
Todos los 1520 archivos irán al mismo directorio.

Ahora en su directorio actual `data-shell`,
Nelle puede ver qué archivos tiene usando el comando:

~~~
$ ls north-pacific-gyre/2012-07-03/
~~~
{: .bash}

Esto es mucho para escribir,
Pero puede permitir que la cáscara haga la mayor parte del trabajo a través de lo que se llama ** tab completion **.
Si escribe:

~~~
$ ls nor
~~~
{: .bash}

Y después presiona la lengüeta (la llave de la lengüeta en su teclado),
La shell completa automáticamente el nombre del directorio para ella:

~~~
$ ls north-pacific-gyre/
~~~
{: .bash}

Si presiona la lengüeta otra vez,
Bash añadirá `2012-07-03 /` al comando,
Ya que es la única conclusión posible.
Presionar la pestaña de nuevo no hace nada,
Ya que hay 19 posibilidades;
Presionando la lengüeta dos veces trae para arriba una lista de todos los archivos,
y así.
Esto se denomina ** tab completion **,
Y lo veremos en muchas otras herramientas a medida que avancemos.

> ## Rutas Absolutas vs Relativas
>
> Starting from `/Users/amanda/data/`,
> which of the following commands could Amanda use to navigate to her home directory,
> which is `/Users/amanda`?
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## Solution
> > 1. No: `.` stands for the current directory.
> > 2. No: `/` stands for the root directory.
> > 3. No: Amanda's home directory is `/Users/amanda`.
> > 4. No: this goes up two levels, i.e. ends in `/Users`.
> > 5. Yes: `~` stands for the user's home directory, in this case `/Users/amanda`.
> > 6. No: this would navigate into a directory `home` in the current directory if it exists.
> > 7. Yes: unnecessarily complicated, but correct.
> > 8. Yes: shortcut to go back to the user's home directory.
> > 9. Yes: goes up one level.
> {: .solution}
{: .challenge}

> ## Relative Path Resolution
>
> Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
> what will `ls ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original pnas_final pnas_sub`
>
> ![File System for Challenge Questions](../fig/filesystem-challenge.svg)
>
> > ## Solution
> > 1. No: there *is* a directory `backup` in `/Users`.
> > 2. No: this is the content of `Users/thing/backup`,
> >    but with `..` we asked for one level further up.
> > 3. No: see previous explanation.
> >    Also, we did not specify `-F` to display `/` at the end of the directory names.
> > 4. Yes: `../backup` refers to `/Users/backup`.
> {: .solution}
{: .challenge}

> ## `ls` Reading Comprehension
>
> Assuming a directory structure as in the above Figure
> (File System for Challenge Questions), if `pwd` displays `/Users/backup`,
> and `-r` tells `ls` to display things in reverse order,
> what command will display:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
> 4.  Either #2 or #3 above, but not #1.
>
> > ## Solution
> >  1. No: `pwd` is not the name of a directory.
> >  2. Yes: `ls` without directory argument lists files and directories
> >     in the current directory.
> >  3. Yes: uses the absolute path explicitly.
> >  4. Correct: see explanations above.
> {: .solution}
{: .challenge}

> ## Exploring More `ls` Arguments
>
> What does the command `ls` do when used with the `-l` and `-h` arguments?
>
> Some of its output is about properties that we do not cover in this lesson (such
> as file permissions and ownership), but the rest should be useful
> nevertheless.
>
> > ## Solution
> > The `-l` arguments makes `ls` use a **l**ong listing format, showing not only
> > the file/directory names but also additional information such as the file size
> > and the time of its last modification. The `-h` argument makes the file size
> > "**h**uman readable", i.e. display something like `5.3K` instead of `5369`.
> {: .solution}
{: .challenge}

> ## Listing Recursively and By Time
>
> The command `ls -R` lists the contents of directories recursively, i.e., lists
> their sub-directories, sub-sub-directories, and so on in alphabetical order
> at each level. The command `ls -t` lists things by time of last change, with
> most recently changed files or directories first.
> In what order does `ls -R -t` display things? Hint: `ls -l` uses a long listing
> format to view timestamps.
>
> > ## Solution
> > The directories are listed alphabetical at each level, the files/directories
> > in each directory are sorted by time of last change.
> {: .solution}
{: .challenge}
