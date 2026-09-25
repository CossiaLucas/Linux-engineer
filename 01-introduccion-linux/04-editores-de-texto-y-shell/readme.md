# Editores de texto y Shell

## Editores de texto

La edición de archivos de texto es fundamental, tanto para administrar Linux como para el desarrollo de aplicaciones en distintos lenguajes. En
Linux contamos con una variedad de editores, algunos de ellos son:

* `emacs` => Es un programa muy adaptable que puede realizar muchas más tareas además de editar texto.
* `nano` => Surgió como un reemplazo libre del editor de texto llamado `Pico`. De hecho si hoy en dia usamos el comando `pico`, levantara el archivo con `nano`
* `Vi y VIM` => La primera, presente en casi todos los sistemas UNIX y la segunda, su versión mejorada.

### Vi y VIM

Vi (`Visual`) es un programa informático que entra en la categoría de editores de texto. Esto se debe a que, a diferencia de un procesador de texto, no
ofrece herramientas para determinar visualmente cómo quedará el documento impreso.
Es por esto que carece de opciones como centrado o justificación de párrafos, pero permite mover, copiar, eliminar o insertar caracteres con
mucha versatilidad. Este tipo de programas es frecuente mente utilizado por programadores para escribir código fuente de software.

VIM (`Vi IMproved`) es una versión mejorada del editor de texto Vi, presente en todos los sistemas UNIX

Estos editores de texto son importantes porque se encuentran en todos los sistemas Unix, y eso
los convierte en herramientas fundamentales del administrador. Poseen características que
brindan versatilidad a la hora de editar archivos de texto en forma rápida.

Si el archivo no existe, el comando lo crea como un documento en blanco y lo abre. En cambio, si ya existe, lo abre y muestra su contenido.

Vi es un editor con diferentes **modos de operación**. En el *modo de edición*, se podrá agregar un texto a un archivo. En *modo de comandos* las teclas que se oprimen pueden
representar algún comando de Vi. Cuando se comience a editar un texto, Vi estará en modo para ingresar comandos.

### Modo comandos

Varios comandos están disponibles directamente, con solo apretar una o dos teclas, y otros están
disponibles en el modo ***last line*** o última línea, al que se accede presionando la tecla dos puntos `:` y, seguido a esto, se indica la acción o comando a ejecutar. 
Para salir del modo de última línea, podemos utilizar la tecla `ESC`.

### Algunos comandos

| Comando | Descripción |
|---|---|
| `0` | (Cero) Inicio de la línea |
| `$` | Fin de la línea |
| `w` | Adelante una palabra |
| `W` | Adelante una palabra incluyendo puntuación |
| `b` | Atrás una palabra |
| `B` | Atrás una palabra incluyendo puntuación |
| `e` | Al final de la palabra actual |
| `E` | Al final de la palabra actual incluyendo puntuación |
| `n-` | Arriba `n` líneas, primer carácter no espacio |
| `n+` | Abajo `n` líneas, primer carácter no espacio |
| `H` | Primera línea de la pantalla actual |
| `M` | Línea a mitad de la pantalla actual |
| `L` | Última línea de la pantalla actual |
| `yy` | Copia la línea actual |
| `Y` | Copia la línea actual |
| `nyy` | Copia `n` líneas |
| `yw` | Copia la palabra actual |
| `dd` | Corta la línea actual (también se usa para borrar) |
| `p` | Pega después del cursor |
| `P` | Pega antes del cursor |
| `a` | Inserta texto después del cursor |
| `A` | Inserta texto al final de la línea actual |
| `i` | Inserta texto antes del cursor |
| `I` | Inserta texto antes del primer carácter no espacio de la línea actual |
| `o` | Abre una nueva línea después de la actual |
| `O` | Abre una nueva línea antes de la actual |
| `r` | Reemplaza el carácter actual |
| `R` | Reemplaza el carácter actual y los siguientes hasta presionar `ESC` |
| `x` | Borra el carácter en el que está el cursor |
| `X` | Borra el carácter antes del cursor |
| `nx` | Borra `n` caracteres |
| `dd` | Borra la línea actual |
| `ndd` | Borra `n` líneas |
| `dw` | Borra la palabra actual |
| `ndw` | Borra `n` palabras |
| `D` | Borra desde el cursor hasta el final de la línea |
| `dL` | Borra desde el cursor hasta el final de la pantalla |
| `dG` | Borra desde el cursor hasta el final del documento |
| `cw` | Reemplaza la palabra actual con nuevo texto |
| `J` | Junta la línea actual con la siguiente |
| `~` | Cambia mayúscula/minúscula del carácter actual |
| `u` | Deshace el último cambio de texto |
| `U` | Deshace los cambios en la línea actual |
| `.` | Repite el último cambio de texto realizado |
| `>` | Tabula la línea actual hacia la derecha |
| `<` | Tabula la línea actual hacia la izquierda |

### Navegacion 

Para desplazarse en este modo sobre el archivo, se emplean las teclas `j` (abajo), `k` (arriba), `h` (izquierda) y `l` (derecha). 
También se pueden usar las flechas, pero solo si nuestra terminal lo permite.

Avanzar página `Ctrl+U` (PgUp), retroceder página `Ctrl+D` (PgDn). 
Para ir a una línea específica, podemos escribir el número de la línea, seguido de gg o G; por ejemplo 25G, o utilizar “:” seguido
del número de línea (:25) y ENTER

Para buscar un texto: `/texto-a-buscar`, seguido del texto que deseamos buscar y **ENTER**. 
Luego podemos presionar n o N para el siguiente o ante rior resultado de la búsqueda. 

Para salir, se tipea la tecla `ESC` y luego se escribe `:q!` y asi se sale sin guardar los cambios. 

Para guardar los cambios `:w`; para guardar y salir, `:wq`.

Para ejecutar un comando del intérprete de comandos utilizamos `:!` seguido del comando y **ENTER** (ej `:!ls`). Podemos teclear `:set all` para ver las opciones disponibles.

### Comparacion de archivos

Se pueden comparar dos archivos usando `VIM` de la siguiente manera:
En cuyo caso se mostrarán en colores las diferencias entre ambos archivos.

```
vim -d archivo1.txt archivo2.txt
```

### Modo insercion o edicion

Este modo se utiliza para trabajar en el documento y realizar acciones como escribir y editar.
Para ingresar al modo de inserción se usa generalmente la letra “i”. Para regresar al **modo de comandos** se presiona la tecla `ESC`.

### Modo Visual

Se puede pulsar la `v` y utilizar las flechas para moverse por el documento y seleccionar texto
que luego será copiado, borrado, tabulado u otras acciones.

Presentemos algun ejemplo sencilo;

```
# vi ejemplo
```

Despues podemos ver una pantalla similar a esto, si el archivo esta vacio.

```
-
-
-
-
-
```

--- 

## Shell e interpretacion de comandos 

