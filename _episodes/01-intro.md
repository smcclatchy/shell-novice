---
title: "Introducing the Shell"
teaching: 5
exercises: 0
questions:
- "What is a command shell and why would I use one?"
objectives:
- "Explain how the shell relates to the keyboard, the screen, the operating system, and users' programs."
- "Explain when and why command-line interfaces should be used instead of graphical interfaces."
keypoints:
- "Explain the similarities and differences between a file and a directory."
- "Translate an absolute path into a relative path and vice versa."
- "Construct absolute and relative paths that identify specific files and directories."
- "Explain the steps in the shell's read-run-print cycle."
- "Identify the actual command, flags, and filenames in a command-line call."
- "Demonstrate the use of tab completion, and explain its advantages."
keypoints:
- "A shell is a program whose primary purpose is to read commands and run other programs."
- "The shell's main advantages are its high action-to-keystroke ratio, its support for automating repetitive tasks, and that it can be used to access networked machines."
- "The shell's main disadvantages are its primarily textual nature and how cryptic its commands and operation can be."
---

En un nivel alto, las computadoras hacen cuatro cosas:

- ejecutar programas
- Almacenamiento de datos
- comunicarse entre sí
- e interactuar con nosotros

Pueden hacer el último de estos en muchas maneras diferentes,
Incluyendo interfaces cerebro-computadora y de habla directas.
Dado que estas interfaces de hardware están todavía en su infancia,
Todavía tenemos que confiar en pantallas, ratones, touchpads y teclados.
Aunque la mayoría de los modernos sistemas operativos de escritorio se comunican con sus usuarios
Medios de ventanas, iconos y punteros, estas tecnologías de software no se
Difundida hasta los años ochenta. Las raíces de tales * interfaces gráficas de usuario * regresan
Trabajo de Doug Engelbart en los años sesenta, que se puede ver en lo que se ha
Llamada "[La madre de todas las demostraciones] (http://www.youtube.com/watch?v=a11JDLBXtPQ)".

Volviendo aún más lejos,
La única manera de interactuar con las primeras computadoras era volver a conectarlas.
Pero en el medio,
Desde la década de 1950 hasta la década de 1980,
La mayoría de las personas utilizan impresoras de línea.
Estos dispositivos sólo permiten la entrada y salida de las letras, números y puntuación que se encuentran en un teclado estándar,
Así que los lenguajes de programación y las interfaces de software tenían que diseñarse alrededor de esa restricción.

Este tipo de interfaz se denomina
** interfaz de línea de comandos ** o CLI,
Para distinguirlo de un
** interfaz gráfica de usuario ** o GUI,
Que la mayoría de la gente ahora usa.
El corazón de una CLI es un ** bucle de lectura-evaluación-impresión ** o REPL:
Cuando el usuario escribe un comando y presiona la tecla Enter (o Return)
El ordenador lo lee,
Lo ejecuta,
E imprime su salida.
El usuario entonces escribe otro comando,
Y así sucesivamente hasta que el usuario cierre la sesión.

Esta descripción hace que suene como si el usuario envía comandos directamente a la computadora,
Y el ordenador envía la salida directamente al usuario.
De hecho,
Por lo general hay un programa en medio llamado un
** shell de comandos **.
Lo que el usuario escribe en el shell,
Que luego calcula qué comandos ejecutar y ordena al equipo para ejecutarlos.
(Tenga en cuenta que el shell se llama "el shell" porque encierra el sistema operativo
Con el fin de ocultar algo de su complejidad y hacer más fácil interactuar con él.)

Un shell es un programa como cualquier otro.
Lo que es especial es que su trabajo es ejecutar otros programas
En lugar de hacer los cálculos en sí.
El shell Unix más popular es Bash,
El Bourne Again SHell
(Así llamado porque se deriva de una cáscara escrita por Stephen Bourne).
Bash es el shell por defecto en la mayoría de las implementaciones modernas de Unix
Y en la mayoría de los paquetes que proporcionan herramientas similares a Unix para Windows.

Utilizar Bash o cualquier otro shell
A veces se siente más como la programación que como usar un ratón.
Los comandos son tersos (a menudo sólo un par de caracteres de largo),
Sus nombres son frecuentemente crípticos,
Y su salida es líneas de texto en lugar de algo visual como un gráfico.
Por otra parte,
Con sólo unas pocas teclas, el shell nos permite combinar las herramientas existentes en
Potentes oleoductos y manejar grandes volúmenes de datos automáticamente. Esta automatización
No sólo nos hace más productivos, sino que también mejora la reproducibilidad de nuestros flujos de trabajo
Permitiéndonos repetirlas con pocos comandos simples.
Además, la línea de comandos es a menudo la forma más fácil de interactuar con máquinas remotas y superordenadores.
La familiaridad con el shell es casi esencial para ejecutar una variedad de herramientas y recursos especializados
Incluyendo sistemas de computación de alto rendimiento.
A medida que los clusters y los sistemas de computación en la nube se vuelven más populares para el crujido de datos científicos,
Ser capaz de interactuar con ellos se está convirtiendo en una habilidad necesaria.
Podemos aprovechar las habilidades de línea de comandos cubiertas aquí
Para abordar una amplia gama de cuestiones científicas y desafíos computacionales.

## Pipeline de Nelle: Punto de partida

Nelle Nemo, una bióloga marina,
Acaban de regresar de una encuesta de seis meses
[Gyre del Pacífico Norte] (http://en.wikipedia.org/wiki/North_Pacific_Gyre),
Donde ha estado probando la vida marina gelatinosa en la
[Great Pacific Garbage Patch] (http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
Ella tiene 1520 muestras en total, y ahora necesita:

1. Ejecutar cada muestra a través de una máquina de ensayo
    Que medirán la abundancia relativa de 300 proteínas diferentes.
    La salida de la máquina para una sola muestra es
    Un archivo con una línea para cada proteína.
2. Calcular las estadísticas de cada una de las proteínas por separado
    Usando un programa que su supervisor escribió llamado `goostat`.
3. Compare las estadísticas de cada proteína
    Con las estadísticas correspondientes para cada otra proteína
    Utilizando un programa que uno de los otros estudiantes graduados escribió llamado `goodiff`.
4. Anote los resultados.
    Su supervisor realmente le gustaría que lo hiciera a finales de mes
    Para que su papel pueda aparecer en un próximo número especial de * Aquatic Goo Letters *.

La máquina de ensayo tarda aproximadamente media hora en procesar cada muestra.
La buena noticia es que
Sólo se necesitan dos minutos para configurar cada uno.
Dado que su laboratorio tiene ocho máquinas de ensayo que puede utilizar en paralelo,
Este paso "sólo" durará unas dos semanas.

La mala noticia es que si tiene que ejecutar `goostat
