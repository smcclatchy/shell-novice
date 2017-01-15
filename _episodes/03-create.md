---
title: "Working With Files and Directories"
teaching: 15
exercises: 0
questions:
- "How can I create, copy, and delete files and directories?"
- "How can I edit files?"
objectives:
- "Create a directory hierarchy that matches a given diagram."
- "Create files in that hierarchy using an editor or by copying and renaming existing files."
- "Display the contents of a directory using the command line."
- "Delete specified files and/or directories."
keypoints:
- "`cp old new` copies a file."
- "`mkdir path` creates a new directory."
- "`mv old new` moves (renames) a file or directory."
- "`rm path` removes (deletes) a file."
- "`rmdir path` removes (deletes) an empty directory."
- "Use of the Control key may be described in many ways, including `Ctrl-X`, `Control-X`, and `^X`."
- "The shell does not have a trash bin: once something is deleted, it's really gone."
- "Nano is a very simple text editor: please use something else for real work."
---

Ahora sabemos cómo explorar archivos y directorios,
Pero ¿cómo los creamos en primer lugar?
Volvamos a nuestro directorio `data-shell` en el Escritorio
Y use `ls -F` para ver lo que contiene:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

~~~
$ ls -F
~~~
{: .bash}

~~~
creatures/  molecules/           pizza.cfg
data/       north-pacific-gyre/  solar.pdf
Desktop/    notes.txt            writing/
~~~
{: .output}

Vamos a crear un nuevo directorio llamado `thesis` usando el comando` mkdir thesis`
(Que no tiene salida):

~~~
$ mkdir thesis
~~~
{: .bash}

Como usted puede adivinar de su nombre,
`Mkdir` significa" make directory ".
Dado que `thesis` es una trayectoria relativa
(Es decir, no tiene una barra inclinada principal),
El nuevo directorio se crea en el directorio de trabajo actual:

~~~
$ ls -F
~~~
{: .bash}

~~~
creatures/  north-pacific-gyre/  thesis/
data/       notes.txt            writing/
Desktop/    pizza.cfg
molecules/  solar.pdf
~~~
{: .output}

> ## Dos maneras de hacer lo mismo
> Usar el shell para crear un directorio no es diferente al usar un explorador de archivos.
> Si abre el directorio actual utilizando el explorador de archivos gráfico de su sistema operativo,
> El directorio `thesis` aparecerá allí también.
> Si bien son dos formas diferentes de interactuar con los archivos,
> Los archivos y los directorios mismos son iguales.
{: .callout}

> ## Buen nombre para archivos y directorios
>
> Complicado nombres de archivos y directorios pueden hacer su vida muy dolorosa
> Cuando se trabaja en la línea de comandos. Aquí ofrecemos algunos útiles
> Consejos para los nombres de sus archivos de ahora en adelante.
>
> 1. No use espacios en blanco.
>
> Los espacios en blanco pueden hacer un nombre más significativo
> Pero desde whitespace se utiliza para romper los argumentos en la línea de comandos
> Es mejor evitarlos en nombre de archivos y directorios.
> Puede utilizar `-` o` _` en lugar de espacios en blanco.
>
> 2. No comience el nombre con `-` (guión).
>
> Los comandos tratan los nombres que comienzan con `-` como opciones.
>
> 3. Pegue con letras, números, `.` (punto),` -` (guión) y `_` (guión bajo).
>
> Muchos otros personajes tienen un significado especial en la línea de comandos
> Que aprenderemos durante esta lección. Algunos sólo harán que su comando no funcione,
> Pero algunos de ellos pueden incluso hacer que pierdas algunos datos!
>
> Si necesita referirse a nombres de archivos o directorios que tengan espacios en blanco
> U otro carácter no alfanumérico, debe rodear el nombre entre comillas (`" "`).
{: .callout}

Puesto que acabamos de crear el directorio `thesis`, no hay nada en él todavía:

~~~
$ ls -F thesis
~~~
{: .bash}

Cambiemos nuestro directorio de trabajo a `thesis` usando` cd`,
A continuación, ejecute un editor de texto llamado Nano para crear un archivo denominado `draft.txt`:

~~~
$ cd thesis
$ nano draft.txt
~~~
{: .bash}

> ## ¿Qué Editor?
>
> Cuando decimos, "` nano` es un editor de texto, "realmente queremos decir" texto ": puede
> Sólo funciona con datos de caracteres simples, no con tablas, imágenes o cualquier otro
> Medios amigables para el hombre. Lo utilizamos en ejemplos porque casi nadie puede
> Conducirlo en cualquier lugar sin entrenamiento, pero por favor use algo más
> Potente para el trabajo real. En los sistemas Unix (como Linux y Mac OS X)
> Muchos programadores utilizan [Emacs] (http://www.gnu.org/software/emacs/) o
> [Vim] (http://www.vim.org/) (ambos de los cuales son completamente intuitivos,
> Incluso por estándares Unix), o un editor gráfico como
> [Gedit] (http://projects.gnome.org/gedit/). En Windows, es posible que desee
> Utilice [Notepad ++] (http://notepad-plus-plus.org/). Windows también tiene un built-in
> Editor llamado `notepad` que se puede ejecutar desde la línea de comandos en el mismo
> Way como `nano` para los propósitos de esta lección.
>
> Sea cual sea el editor que uses, necesitarás saber dónde busca
> Para y guarda archivos. Si lo inicia desde el shell, será (probablemente)
> Utilice su directorio de trabajo actual como su ubicación predeterminada. Si utiliza
> El menú de inicio de su computadora, puede guardar archivos en su escritorio o
> Directorio de documentos en su lugar. Puede cambiar esto navegando a
> Otro directorio la primera vez que "Guardar como ..."
{: .callout}

Escriba algunas líneas de texto.
Una vez que estamos contentos con nuestro texto, podemos presionar `Ctrl-O` (presionar la tecla Ctrl o Control y, mientras
Mantenga pulsada la tecla O) para escribir nuestros datos en el disco
(Se nos preguntará en qué archivo queremos guardar esto:
Pulse Intro para aceptar el valor predeterminado sugerido de `draft.txt`).

![Nano in Action](../fig/nano-screenshot.png)

Una vez que se guarda nuestro archivo, podemos usar `Ctrl-X` para salir del editor y
Volver a la cáscara.

> ## Control, Ctrl o ^ Tecla
>
> La tecla Control también se denomina tecla "Ctrl". Hay varias maneras
> En la que puede ser descrita la tecla Control. Por ejemplo, puede
> Ver una instrucción para presionar la tecla Control y, mientras la mantiene pulsada,
> Presione la tecla X, descrita como cualquiera de:
>
> * `Control-X`
> * `Control+X`
> * `Ctrl-X`
> * `Ctrl+X`
> * `^X`
>
> En nano, a lo largo de la parte inferior de la pantalla verá `^ G Obtener Ayuda ^ O WriteOut`.
> Esto significa que puede usar `Control-G` para obtener ayuda y` Control-O` para guardar su
> Archivo.
{: .callout}

`Nano` no deja ninguna salida en la pantalla después de que salga,
Pero `ls` ahora muestra que hemos creado un archivo llamado` draft.txt`:

~~~
$ ls
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

Arreglémonos ejecutando `rm draft.txt`:

~~~
$ rm draft.txt
~~~
{: .bash}

Este comando elimina los archivos (`rm` es el término" remove ").
Si ejecutamos `ls` de nuevo,
Su salida está vacía una vez más,
Que nos dice que nuestro archivo ha desaparecido:

~~~
$ ls
~~~
{: .bash}

> ## Eliminar es para siempre
>
> La shell de Unix no tiene un contenedor de basura que podamos recuperar eliminado
> De los archivos de (aunque la mayoría de las interfaces gráficas a Unix hacer). En lugar,
> Cuando eliminamos archivos, se desenganchan del sistema de archivos para que
> Su espacio de almacenamiento en disco puede ser reciclado. Herramientas para encontrar y
> La recuperación de los archivos eliminados existe, pero no hay garantía de que
> Trabajar en cualquier situación particular, ya que la computadora puede reciclar la
> El espacio en disco del archivo inmediatamente.
{: .callout}

Vamos a volver a crear ese archivo
Y luego sube un directorio a `/ Users / nelle / Desktop / data-shell` usando `cd ..`:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell/thesis
~~~
{: .output}

~~~
$ nano draft.txt
$ ls
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

~~~
$ cd ..
~~~
{: .bash}

Si tratamos de eliminar todo el directorio `thesis` usando `rm thesis`,
Recibimos un mensaje de error:

~~~
$ rm thesis
~~~
{: .bash}

~~~
rm: cannot remove `thesis': Is a directory
~~~
{: .error}

This happens because `rm` by default only works on files, not directories.

To really get rid of `thesis` we must also delete the file `draft.txt`.
We can do this with the [recursive](https://en.wikipedia.org/wiki/Recursion) option for `rm`:

~~~
$ rm -r thesis
~~~
{: .bash}

> ## With Great Power Comes Great Responsibility
>
> Removing the files in a directory recursively can be very dangerous
> operation. If we're concerned about what we might be deleting we can
> add the "interactive" flag `-i` to `rm` which will ask us for confirmation
> before each step
>
> ~~~
> $ rm -r -i thesis
> rm: descend into directory ‘thesis’? y
> rm: remove regular file ‘thesis/draft.txt’? y
> rm: remove directory ‘thesis’? y
> ~~~
> {: .bash}
>
> This removes everything in the directory, then the directory itself, asking
> at each step for you to confirm the deletion.
{: .callout}

Let's create that directory and file one more time.
(Note that this time we're running `nano` with the path `thesis/draft.txt`,
rather than going into the `thesis` directory and running `nano` on `draft.txt` there.)

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

~~~
$ mkdir thesis
$ nano thesis/draft.txt
$ ls thesis
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

`draft.txt` isn't a particularly informative name,
so let's change the file's name using `mv`,
which is short for "move":

~~~
$ mv thesis/draft.txt thesis/quotes.txt
~~~
{: .bash}

The first parameter tells `mv` what we're "moving",
while the second is where it's to go.
In this case,
we're moving `thesis/draft.txt` to `thesis/quotes.txt`,
which has the same effect as renaming the file.
Sure enough,
`ls` shows us that `thesis` now contains one file called `quotes.txt`:

~~~
$ ls thesis
~~~
{: .bash}

~~~
quotes.txt
~~~
{: .output}

One has to be careful when specifying the target file name, since `mv` will
silently overwrite any existing file with the same name, which could
lead to data loss. An additional flag, `mv -i` (or `mv --interactive`),
can be used to make `mv` ask you for confirmation before overwriting.

Just for the sake of consistency,
`mv` also works on directories --- there is no separate `mvdir` command.

Let's move `quotes.txt` into the current working directory.
We use `mv` once again,
but this time we'll just use the name of a directory as the second parameter
to tell `mv` that we want to keep the filename,
but put the file somewhere new.
(This is why the command is called "move".)
In this case,
the directory name we use is the special directory name `.` that we mentioned earlier.

~~~
$ mv thesis/quotes.txt .
~~~
{: .bash}

The effect is to move the file from the directory it was in to the current working directory.
`ls` now shows us that `thesis` is empty:

~~~
$ ls thesis
~~~
{: .bash}

Further,
`ls` with a filename or directory name as a parameter only lists that file or directory.
We can use this to see that `quotes.txt` is still in our current directory:

~~~
$ ls quotes.txt
~~~
{: .bash}

~~~
quotes.txt
~~~
{: .output}

The `cp` command works very much like `mv`,
except it copies a file instead of moving it.
We can check that it did the right thing using `ls`
with two paths as parameters --- like most Unix commands,
`ls` can be given multiple paths at once:

~~~
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .bash}

~~~
quotes.txt   thesis/quotations.txt
~~~
{: .output}

To prove that we made a copy,
let's delete the `quotes.txt` file in the current directory
and then run that same `ls` again.

~~~
$ rm quotes.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .bash}

~~~
ls: cannot access quotes.txt: No such file or directory
thesis/quotations.txt
~~~
{: .error}

This time it tells us that it can't find `quotes.txt` in the current directory,
but it does find the copy in `thesis` that we didn't delete.

> ## What's In A Name?
>
> You may have noticed that all of Nelle's files' names are "something dot
> something", and in this part of the lesson, we always used the extension
> `.txt`.  This is just a convention: we can call a file `mythesis` or
> almost anything else we want. However, most people use two-part names
> most of the time to help them (and their programs) tell different kinds
> of files apart. The second part of such a name is called the
> **filename extension**, and indicates
> what type of data the file holds: `.txt` signals a plain text file, `.pdf`
> indicates a PDF document, `.cfg` is a configuration file full of parameters
> for some program or other, `.png` is a PNG image, and so on.
>
> This is just a convention, albeit an important one. Files contain
> bytes: it's up to us and our programs to interpret those bytes
> according to the rules for plain text files, PDF documents, configuration
> files, images, and so on.
>
> Naming a PNG image of a whale as `whale.mp3` doesn't somehow
> magically turn it into a recording of whalesong, though it *might*
> cause the operating system to try to open it with a music player
> when someone double-clicks it.
{: .callout}

> ## Renaming Files
>
> Suppose that you created a `.txt` file in your current directory to contain a list of the
> statistical tests you will need to do to analyze your data, and named it: `statstics.txt`
>
> After creating and saving this file you realize you misspelled the filename! You want to
> correct the mistake, which of the following commands could you use to do so?
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`
>
> > ## Solution
> > 1. No.  While this would create a file with the correct name, the incorrectly named file still exists in the directory
> > and would need to be deleted.
> > 2. Yes, this would work to rename the file.
> > 3. No, the period(.) indicates where to move the file, but does not provide a new file name; identical file names
> > cannot be created.
> > 4. No, the period(.) indicates where to copy the file, but does not provide a new file name; identical file names
> > cannot be created.
> {: .solution}
{: .challenge}

> ## Moving and Copying
>
> What is the output of the closing `ls` command in the sequence shown below?
>
> ~~~
> $ pwd
> ~~~
> {: .bash}
> ~~~
> /Users/jamie/data
> ~~~
> {: .output}
> ~~~
> $ ls
> ~~~
> {: .bash}
> ~~~
> proteins.dat
> ~~~
> {: .output}
> ~~~
> $ mkdir recombine
> $ mv proteins.dat recombine
> $ cp recombine/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
> {: .bash}
>
> 1.   `proteins-saved.dat recombine`
> 2.   `recombine`
> 3.   `proteins.dat recombine`
> 4.   `proteins-saved.dat`
>
> > ## Solution
> > We start in the `/Users/jamie/data` directory, and create a new folder called `recombine`.
> > The second line moves (`mv`) the file `proteins.dat` to the new folder (`recombine`).
> > The third line makes a copy of the file we just moved.  The tricky part here is where the file was
> > copied to.  Recall that `..` means "go up a level", so the copied file is now in `/Users/jamie`.
> > Notice that `..` is interpreted with respect to the current working
> > directory, **not** with respect to the location of the file being copied.
> > So, the only thing that will show using ls (in `/Users/jamie/data`) is the recombine folder.
> >
> > 1. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> > 2. Yes
> > 3. No, see explanation above.  `proteins.dat` is located at `/Users/jamie/data/recombine`
> > 4. No, see explanation above.  `proteins-saved.dat` is located at `/Users/jamie`
> {: .solution}
{: .challenge}

> ## Organizing Directories and Files
>
> Jamie is working on a project and she sees that her files aren't very well
> organized:
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> analyzed/  fructose.dat    raw/   sucrose.dat
> ~~~
> {: .output}
>
> The `fructose.dat` and `sucrose.dat` files contain output from her data
> analysis. What command(s) covered in this lesson does she need to run so that the commands below will
> produce the output shown?
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> analyzed/   raw/
> ~~~
> {: .output}
> ~~~
> $ ls analyzed
> ~~~
> {: .bash}
> ~~~
> fructose.dat    sucrose.dat
> ~~~
> {: .output}
{: .challenge}

> ## Copy with Multiple Filenames
>
> What does `cp` do when given several filenames and a directory name, as in:
>
> ~~~
> $ mkdir backup
> $ cp thesis/citations.txt thesis/quotations.txt backup
> ~~~
> {: .bash}
>
> What does `cp` do when given three or more filenames, as in:
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> intro.txt    methods.txt    survey.txt
> ~~~
> {: .output}
> ~~~
> $ cp intro.txt methods.txt survey.txt
> ~~~
> {: .bash}
{: .challenge}

> ## Listing Recursively and By Time
>
> The command `ls -R` lists the contents of directories recursively,
> i.e., lists their sub-directories, sub-sub-directories, and so on
> in alphabetical order at each level.
> The command `ls -t` lists things by time of last change,
> with most recently changed files or directories first.
> In what order does `ls -R -t` display things?
{: .challenge}

> ## Creating Files a Different Way
>
> We have seen how to create text files using the `nano` editor.
> Now, try the following command in your home directory:
>
> ~~~
> $ cd                  # go to your home directory
> $ touch my_file.txt
> ~~~
> {: .bash}
>
> 1.  What did the touch command do?
>     When you look at your home directory using the GUI file explorer,
>     does the file show up?
>
> 2.  Use `ls -l` to inspect the file's.  How large is `my_file.txt`?
>
> 3.  When might you want to create a file this way?
{: .challenge}

> ## Moving to the Current Folder
>
> After running the following commands,
> Jamie realizes that she put the files `sucrose.dat` and `maltose.dat` into the wrong folder:
>
> ~~~
> $ ls -F
> raw/ analyzed/
> $ ls -F analyzed
> fructose.dat glucose.dat maltose.dat sucrose.dat
> $ cd raw/
> ~~~
> {: .bash}
>
> Fill in the blanks to move these files to the current folder
> (i.e., the one she is currently in):
>
> ~~~
> $ mv ___/sucrose.dat  ___/maltose.dat ___
> ~~~
> {: .bash}
{: .challenge}

> ## Using `rm` Safely
>
> What happens when we type `rm -i thesis/quotations.txt`?
> Why would we want this protection when using `rm`?
>
> > ## Solution
> >
> > Ask for confirmation.
> {: .solution}
{: .challenge}

> ## Copy a folder structure sans files
>
> You're starting a new experiment, and would like to duplicate the file
> structure from your previous experiment without the data files so you can
> add new data.
>
> Assume that the file structure is in a folder called '2016-05-18-data',
> which contains folders named 'raw' and 'processed' that contain data files.
> The goal is to copy the file structure of the `2016-05-18-data` folder
> into a folder called `2016-05-20-data` and remove the data files from
> the directory you just created.
>
> Which of the following set of commands would achieve this objective?
> What would the other commands do?
>
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm 2016-05-20-data/data/raw/*
> $ rm 2016-05-20-data/data/processed/*
> ~~~
> {: .bash}
> ~~~
> $ rm 2016-05-20-data/data/raw/*
> $ rm 2016-05-20-data/data/processed/*
> $ cp -r 2016-05-18-data/ 2016-5-20-data/
> ~~~
> {: .bash}
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm -r -i 2016-05-20-data/
> ~~~
> {: .bash}
{: .challenge}
