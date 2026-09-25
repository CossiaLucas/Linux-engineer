# **Editores de texto y Shell**

## **Editores de texto**

Linux cuenta con una gran variedad de editores de texto. Algunos de los más conocidos son:

* `emacs` → Es un editor de texto altamente configurable y extensible. Además de editar texto, puede utilizarse para programación, gestión de archivos, terminal y muchas otras tareas mediante extensiones.

* `nano` → Es un editor de texto sencillo y fácil de utilizar desde la terminal. Está basado en el concepto de `Pico`, editor desarrollado originalmente para el sistema de correo electrónico Pine. En algunos sistemas puede existir un alias o enlace que haga que `pico` invoque a `nano`, pero esto depende de la distribución y de su configuración.

* `vi` → Es el editor de texto tradicional de los sistemas Unix y se encuentra disponible en una gran cantidad de sistemas Unix/Linux.

* `vim` → Significa **Vi IMproved** y es una versión mejorada y ampliada de `vi`. Incorpora numerosas características adicionales, como resaltado de sintaxis, múltiples niveles de deshacer, búsqueda y reemplazo avanzado, ventanas divididas y un sistema de extensiones.

Para un administrador de sistemas, conocer al menos los conceptos básicos de `vi`/`vim` resulta especialmente útil porque permite editar archivos incluso cuando se está trabajando de forma remota mediante SSH.

---

## **Vi y Vim**

`vi` es un editor de texto orientado principalmente a la edición eficiente de archivos mediante comandos y atajos de teclado. A diferencia de un procesador de textos, no está orientado a definir visualmente cómo se imprimirá un documento.

Su funcionamiento se basa en comandos que permiten desplazarse por el archivo y realizar operaciones como insertar, modificar, copiar, eliminar y buscar texto.

`Vim` (**Vi IMproved**) es una implementación mejorada de `vi`, compatible con sus comandos tradicionales y con una gran cantidad de funcionalidades adicionales.

Entre sus características se encuentran:

* Edición desde la terminal.
* Búsqueda y reemplazo de texto.
* Resaltado de sintaxis.
* Múltiples niveles de deshacer y rehacer.
* Ventanas y pestañas.
* Soporte para numerosos lenguajes y formatos.
* Sistema de plugins y extensiones.
* Comparación de archivos mediante `vimdiff`.

Esto hace que Vim sea especialmente útil tanto para programación como para la administración de sistemas.

---

# **Modos de operación**

Una de las características más importantes de Vim es que funciona mediante diferentes **modos de operación**. Dependiendo del modo en el que nos encontremos, las mismas teclas pueden tener diferentes funciones.

Los principales modos son:

| Modo             | Función                                                                              |
| ---------------- | ------------------------------------------------------------------------------------ |
| **Normal**       | Permite desplazarse por el archivo y ejecutar comandos de edición.                   |
| **Insert**       | Permite introducir texto normalmente.                                                |
| **Visual**       | Permite seleccionar bloques de texto para realizar operaciones sobre ellos.          |
| **Command-line** | Permite introducir comandos precedidos normalmente por `:` como `:w`, `:q` o `:set`. |

Al iniciar Vim, normalmente se encuentra en **modo Normal**. Desde este modo podemos acceder a los demás modos.

Por ejemplo:

``` text
i       → modo Insert
v       → modo Visual
:       → modo Command-line
ESC     → volver al modo Normal
```

La documentación oficial de Vim distingue estos modos explícitamente, incluyendo Normal, Visual, Insert y Command-line.

---

# **Modo Normal**

El **modo Normal** es el modo principal de Vim. En él, las teclas no se interpretan directamente como texto, sino como comandos.

Por ejemplo:

``` text
h → izquierda
j → abajo
k → arriba
l → derecha
```

También es posible utilizar las teclas de dirección si la terminal las admite correctamente.

Una de las características más importantes de Vim es que muchos comandos pueden combinarse entre sí.

Por ejemplo:

``` text
d + w → dw
```

donde:

* `d` significa **delete** (eliminar).
* `w` significa desplazarse hasta la siguiente palabra.

Por lo tanto:

``` text
dw
```

realiza una operación de eliminación utilizando `w` como movimiento.

Esta combinación de **operadores + movimientos** es uno de los conceptos fundamentales para comprender Vim.

Algunos operadores importantes son:

| Operador | Función               |
| -------- | --------------------- |
| `d`      | Eliminar              |
| `c`      | Cambiar/reemplazar    |
| `y`      | Copiar (yank)         |
| `>`      | Aumentar indentación  |
| `<`      | Disminuir indentación |

Por ejemplo:

``` text
dw  → eliminar hasta el siguiente movimiento de palabra
cw  → cambiar hasta el siguiente movimiento de palabra
yw  → copiar hasta el siguiente movimiento de palabra
```

---

# **Comandos de movimiento y edición**

| Comando | Descripción                                                                            |
| ------- | -------------------------------------------------------------------------------------- |
| `0`     | Ir al inicio de la línea                                                               |
| `$`     | Ir al final de la línea                                                                |
| `w`     | Avanzar al comienzo de la siguiente palabra                                            |
| `W`     | Avanzar al comienzo de la siguiente palabra considerando los espacios como separadores |
| `b`     | Retroceder al comienzo de la palabra anterior                                          |
| `B`     | Retroceder al comienzo de una palabra delimitada por espacios                          |
| `e`     | Ir al final de la palabra actual                                                       |
| `E`     | Ir al final de una palabra delimitada por espacios                                     |
| `n-`    | Subir `n` líneas y situarse en el primer carácter no vacío                             |
| `n+`    | Bajar `n` líneas y situarse en el primer carácter no vacío                             |
| `H`     | Ir a la primera línea visible de la pantalla                                           |
| `M`     | Ir a la línea central de la pantalla                                                   |
| `L`     | Ir a la última línea visible de la pantalla                                            |
| `yy`    | Copiar la línea actual                                                                 |
| `Y`     | Copiar la línea actual                                                                 |
| `nyy`   | Copiar `n` líneas                                                                      |
| `yw`    | Copiar hasta el movimiento `w`                                                         |
| `dd`    | Eliminar la línea actual                                                               |
| `p`     | Pegar después de la posición actual                                                    |
| `P`     | Pegar antes de la posición actual                                                      |
| `a`     | Insertar texto después del cursor                                                      |
| `A`     | Insertar texto al final de la línea                                                    |
| `i`     | Insertar texto antes del cursor                                                        |
| `I`     | Insertar texto al comienzo del primer carácter no vacío de la línea                    |
| `o`     | Abrir una nueva línea debajo de la actual y entrar en modo Insert                      |
| `O`     | Abrir una nueva línea encima de la actual y entrar en modo Insert                      |
| `r`     | Reemplazar el carácter actual                                                          |
| `R`     | Entrar en modo Replace y reemplazar caracteres hasta presionar `ESC`                   |
| `x`     | Eliminar el carácter bajo el cursor                                                    |
| `X`     | Eliminar el carácter anterior al cursor                                                |
| `nx`    | Eliminar `n` caracteres                                                                |
| `ndd`   | Eliminar `n` líneas                                                                    |
| `dw`    | Eliminar utilizando `w` como movimiento                                                |
| `ndw`   | Repetir `dw` `n` veces                                                                 |
| `D`     | Eliminar desde el cursor hasta el final de la línea                                    |
| `dG`    | Eliminar desde el cursor hasta el final del archivo                                    |
| `cw`    | Cambiar el texto utilizando `w` como movimiento                                        |
| `J`     | Unir la línea actual con la siguiente                                                  |
| `~`     | Cambiar entre mayúsculas y minúsculas                                                  |
| `u`     | Deshacer el último cambio                                                              |
| `U`     | Deshacer los cambios realizados en la línea actual                                     |
| `.`     | Repetir el último cambio                                                               |
| `>`     | Aumentar la indentación                                                                |
| `<`     | Disminuir la indentación                                                               |

> **Nota:** `W`, `B` y `E` trabajan con una definición de "palabra" más amplia que `w`, `b` y `e`, donde los espacios actúan como delimitadores principales.

---

# **Navegación**

Para desplazarse por el archivo en modo Normal se pueden utilizar:

``` text
h → izquierda
j → abajo
k → arriba
l → derecha
```

También se pueden utilizar las teclas de dirección.

Para desplazarse verticalmente:

``` text
Ctrl + U → desplazar aproximadamente media pantalla hacia arriba
Ctrl + D → desplazar aproximadamente media pantalla hacia abajo
```

No deben considerarse exactamente equivalentes a `Page Up` y `Page Down`, ya que el desplazamiento corresponde aproximadamente a media pantalla.

También existen:

``` text
Ctrl + B → una pantalla hacia arriba
Ctrl + F → una pantalla hacia abajo
```

---

## **Ir a una línea determinada**

Para ir directamente a una línea determinada podemos utilizar:

``` text
25G
```

Esto lleva el cursor a la línea 25.

También podemos utilizar:

``` text
:25
```

y presionar `ENTER`.

Otros comandos útiles:

``` text
gg → ir al comienzo del archivo
G  → ir al final del archivo
```

Por ejemplo:

``` text
gg
```

lleva el cursor a la primera línea del archivo, mientras que:

``` text
G
```

lo lleva a la última línea.

---

# **Búsqueda de texto**

Para buscar un texto desde el modo Normal se utiliza `/`:

``` text
/texto-a-buscar
```

Después de escribir el texto se presiona `ENTER`.

Por ejemplo:

``` text
/servidor
```

buscará la próxima aparición de `servidor`.

Después de realizar una búsqueda:

``` text
n → siguiente coincidencia
N → coincidencia anterior
```

También se puede utilizar `?` para realizar la búsqueda en dirección inversa:

``` text
?servidor
```

---

# **Guardar y salir**

Los comandos precedidos por `:` se ejecutan desde el modo **Command-line**.

Para entrar en este modo:

``` text
:
```

Algunos de los comandos más importantes son:

| Comando          | Función                              |
| ---------------- | ------------------------------------ |
| `:w`             | Guardar los cambios                  |
| `:q`             | Salir                                |
| `:wq`            | Guardar y salir                      |
| `:x`             | Guardar y salir                      |
| `:q!`            | Salir descartando los cambios        |
| `:w archivo.txt` | Guardar el contenido con otro nombre |

Por ejemplo:

``` text
:wq
```

guarda los cambios y cierra Vim.

En cambio:

``` text
:q!
```

sale del editor descartando los cambios realizados desde la última escritura.

---

# **Ejecutar comandos del Shell desde Vim**

Desde el modo Command-line podemos ejecutar comandos del sistema utilizando `:!`.

Por ejemplo:

``` text
:!ls
```

ejecuta el comando `ls` en el Shell y muestra su resultado.

También podemos ejecutar:

``` text
:!pwd
```

para mostrar el directorio actual.

Otra opción útil es:

``` text
:set all
```

que permite consultar las opciones configurables de Vim.

Vim también dispone de un sistema de ayuda integrado:

``` text
:help
```

Por ejemplo:

``` text
:help dd
```

muestra información sobre el comando `dd`.

También podemos utilizar:

``` text
:help :w
```

para consultar el comando `:w`.

La documentación oficial de Vim recomienda utilizar `:help` y también incluye `vimtutor`, un tutorial interactivo para principiantes.

---

# **Comparación de archivos**

Vim incluye una funcionalidad específica para comparar archivos denominada **Vimdiff**.

Podemos comparar dos archivos utilizando:

``` bash
vim -d archivo1.txt archivo2.txt
```

Por ejemplo:

``` bash
vim -d configuracion_v1.conf configuracion_v2.conf
```

Vim mostrará ambos archivos y resaltará las diferencias entre ellos.

Esta funcionalidad resulta especialmente útil para administradores de sistemas cuando necesitan comparar diferentes versiones de archivos de configuración. Vim incluye `vimdiff` como una de sus herramientas para visualizar diferencias entre archivos.

---

# **Modo Insert**

El modo Insert permite introducir y modificar texto normalmente.

Para entrar en este modo podemos utilizar:

``` text
i
```

Por ejemplo:

``` text
i
```

permite comenzar a escribir en la posición actual del cursor.

Otros comandos habituales son:

``` text
a → insertar después del cursor
A → insertar al final de la línea
o → crear una nueva línea debajo
O → crear una nueva línea encima
```

Para regresar al modo Normal se presiona:

``` text
ESC
```

---

# **Modo Visual**

El modo Visual permite seleccionar una parte del texto para posteriormente realizar operaciones sobre ella.

Para entrar en este modo:

``` text
v
```

Luego podemos desplazarnos con:

``` text
h
j
k
l
```

y seleccionar el texto deseado.

Una vez realizada la selección podemos ejecutar operaciones como:

``` text
y → copiar
d → eliminar
> → aumentar indentación
< → disminuir indentación
```

También existen otras variantes:

``` text
V       → seleccionar líneas completas
Ctrl+V  → seleccionar en bloques rectangulares
```

Para salir del modo Visual podemos presionar:

``` text
ESC
```

---

# **Ejemplo práctico**

Vamos a crear un archivo llamado `ejemplo.txt` utilizando Vim.

Desde el Shell ejecutamos:

``` bash
vim ejemplo.txt
```

Si el archivo no existe, Vim abrirá un buffer nuevo para ese archivo. Al guardar el contenido, se creará `ejemplo.txt`.

La pantalla puede aparecer inicialmente vacía, con las líneas representadas visualmente mediante `~`:

``` text
~
~
~
~
~
~
~
~
```

Ahora podemos presionar:

``` text
i
```

para entrar en modo Insert y escribir:

``` text
Hola mundo!
```

Cuando terminemos de escribir, presionamos:

``` text
ESC
```

para volver al modo Normal.

Luego guardamos el archivo utilizando:

``` text
:wq
```

Desde el Shell podemos comprobar que el archivo fue creado:

``` bash
ls -l ejemplo.txt
```

Y podemos volver a abrirlo:

``` bash
vim ejemplo.txt
```

--- 

# Shell e interpretacion de comandos 

Una vez que el usuario se ha identificado dentro del sistema Linux, se enfrenta con el shell, que aparece simplemente como una interfaz de línea de comandos.

El shell cuenta con un indicador para ingresar comandos llamado prompt, esos comandos son interpretados por el shell y enviados al sistema.
El shell que se esté corriendo en ese momento configura su prompt correspondiente.

La mayoría de los sistemas Linux tienen como predeterminado el shell `bash` que, generalmente,
está configurado para mostrar el nombre de usuario, nombre del servidor y directorio actual de trabajo en el prompt.

``` bash
[lucascossia@debian13 ~]$
```

Existen varios tipos de shell. Difieren ligeramente en sintaxis, funcionalidades y configuración. La mayoría de las distribuciones de Linux usan de manera predeterminada el shell bash. 

## Concatenacion de comandos

Se pueden ejecutar varios comandos, sin que estén conectados entre sí, tipeándolos en la misma línea separados por punto y coma `;`.
Por ejemplo, es posible listar todos los archivos del directorio actual y la fecha de hoy tecleando:

``` bash
ls ; date
```

También podemos ejecutar varios comandos a la vez con los *operadores lógicos* `&&` , `||`. En este caso, solo ejecuta el
comando si cumple con la función lógica dado que, cuando un comando se ejecuta, devuelve un estado `0` bien u otro valor mayor que `0` que se interpreta como **error**.

## Arquitectura

Es importante saber qué tipo de hardware y software se está utilizando, antes de entender y monitorear procesos. 
El comando `uname` nos permite obtener esta información. En la próxima slide se muestra su utilización.

# Comportamiento de bash

Bash usa un conjunto de funciones llamado `readline` al ingresar comandos.
Existen dos modos de operaciones de readline: Vi y Emacs. El nombre Emacs se debe al nombre de un editor que en algún momento fue muy popular en Linux. 
A continuación se verán algunas de sus características.

| Comando    | Funcion  |
| --- | --- |
| `Ctrl + t` | Intercambia la posición del carácter. | 
| `Alt + f`  | Mueve el cursor a la siguiente palabra. |
| `Alt + b`  | Mueve el cursor a la anterior palabra. |
| `Ctrl + u` | Borra la línea actual |

## Completado de comandos/rutas

El shell bash incluye una característica llamada ***completado de comandos***, que permite teclear sólo las primeras letras de un comando o ruta y pulsar la tecla tabulador para que el sistema lo complete.
Si se desea ejecutar el comando dmesg para mostrar el buffer del kernel, se puede teclear:

``` bash
dm
```

Despues presionar `tab`, y el resultado sera:

``` bash
dmesg
```

# Alias

Aunque el sistema operativo y el shell nos ofrecen multitud de comandos y utilidades, podemos crear alias con nombres que tengan más sentido para nosotros o que sean más pequeños y así teclear menos caracteres.

``` bash
alias l. = ls -d .* --color=auto
```

Despues con escribir `l.` se ejecutara todo el comando `'ls -d .* --color=auto`