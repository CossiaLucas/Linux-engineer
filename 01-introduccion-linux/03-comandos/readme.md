# Comandos y directorios

## Directorios

Los directorios son contenedores que van a incluir la información de los objetos, que pueden
ser otros directorios u otros archivos.
En Linux, el directorio de mayor jerarquía es la raíz “/”, a partir de ahí surgen todos los demás
directorios y archivos. Un archivo, por otro lado, es el que va a contener
información; en cambio, un directorio, nos da un orden de cómo vamos a poder ver todos los
archivos y/o directorios.

---

### Inodos

El sistema de archivos identifica a los objetos mediante inodos que contienen información
sobre ellos. Cada uno tiene distintas propiedades que dependiendo de su uso, va a cambiar el valor que contenga.

Podemos usar el comando `stat` para ver esta informacion

```
# stat /etc/resolv.conf
Fichero: /etc/resolv.conf
Tamaño: 125 Bloques: 8 Bloque E/S: 4096 fichero regular
Dispositivo: fd00h/64768d Nodo-i: 1050548 Enlaces: 1
Acceso: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
Acceso: 2019-03-21 14:32:29.532283100 -0300
Modificación: 2019-03-21 14:32:29.432282543 -0300
Cambio: 2019-03-21 14:32:29.439282582 -0300
Creación: -
```

### Rutas absolutas y relativas

Una ruta absoluta comienza desde /, la raíz del sistema.

```
/etc/ssh/sshd_config
```

Una ruta relativa se interpreta tomando como referencia el directorio actual.

```
documentos/archivo.txt
```

---

## FHS

FHS (Filesystem Hierarchy Standard) es un estándar que define la organización y propósito de los principales directorios y archivos de un sistema Unix/Linux.
Los FHS se dividen en dos grupos principalmente:

* Shareables/Unshareables 

El FHS clasifica determinados datos según si podrían ser compartidos entre diferentes sistemas o si están vinculados a una máquina concreta. Esta clasificación es conceptual y no implica que el sistema los comparta automáticamente por red.

* Variables/Static

***Variables.***

    Información que cambia durante el funcionamiento normal del sistema, como logs, cachés, colas de impresión o datos generados por servicios.

***Static.***
    Información cuyo contenido no cambia durante la operación normal del sistema, o que cambia solamente mediante acciones administrativas.

| Directorio | Función                                                                                                     |
| ---------- | ----------------------------------------------------------------------------------------------------------- |
| `/`        | Raíz de la jerarquía del sistema de archivos.                                                               |
| `/boot`    | Archivos necesarios para el proceso de arranque.                                                            |
| `/etc`     | Archivos de configuración específicos del sistema.                                                          |
| `/home`    | Directorios personales de los usuarios.                                                                     |
| `/opt`     | Software adicional instalado como paquetes de terceros.                                                     |
| `/tmp`     | Archivos temporales.                                                                                        |
| `/usr`     | Jerarquía secundaria que contiene gran parte de los programas, bibliotecas y datos compartidos del sistema. |
| `/var`     | Datos variables, como logs, cachés y colas.                                                                 |

### Binarios

Los directorios bin y sbin, se llaman así porque poseen archivos ejecutables. La mayoría están ya compilados como binarios.


FHS sirve como una guía para ubicar y encontrar archivos y directorios, pero eso no garantiza la posibilidad de encontrar cualquier archivo.

| Comando   | Para qué sirve                                                                  |
| --------- | ------------------------------------------------------------------------------- |
| `which`   | Busca ejecutables utilizando las rutas definidas en `PATH`.                     |
| `find`    | Busca archivos y directorios recorriendo el sistema de archivos en tiempo real. |
| `whereis` | Busca la ubicación de un binario, su código fuente y/o sus páginas de manual.   |
| `locate`  | Busca rápidamente utilizando una base de datos previamente indexada.            |


### update db

`updatedb` crea o actualiza la base de datos utilizada por `locate/plocate`.
Ejemplo para actualizar solo lo contenido en un grupo de directorios:

```
sudo updatedb
```
Posteriormente podemos utilizar:
```
locate archivo.txt
```

### Comando file

    `file` analiza el contenido del archivo utilizando información y patrones conocidos para determinar su tipo. Por este motivo, la extensión del archivo no es necesariamente determinante.

**Opciones**
`-f` => Busca por cada línea que hay en el archivo que se le invoque.
`-z` => Intenta ver dentro de los archivos comprimidos.

---

## Manual

Las páginas de manual (`man pages`) son una de las principales fuentes de documentación local de los sistemas Linux. Permiten consultar la descripción, sintaxis, opciones, argumentos, archivos relacionados y comportamiento de numerosos comandos, funciones y componentes del sistema.

| Opción | Descripción |
|---|---|
| `-C configuración` | Indica un fichero de configuración distinto a `/etc/man.conf`. |
| `-M ruta` | Indica en qué directorios se buscarán las páginas man. |
| `-P paginador` | Indica el paginador (programa que da formato y visualiza las páginas). Por defecto es el indicado en la variable de entorno `PAGER`. Los paginadores `more` y `less` son los más utilizados. |
| `-S lista` | Indica una lista de las secciones a buscar separadas por dos puntos (`:`). |
| `-a` | Indica que han de mostrarse todas las entradas coincidentes y no solo la primera. |
| `-c` | Indica que la página fuente ha de ser reformateada. |
| `-d` | Mostrará información de debug en lugar de las páginas man. |
| `-f` | Indica que el programa `man` debe comportarse como el programa `whatis` (se explicará más adelante). |
| `-h` | Muestra información sobre el comando `man`. |
| `-k` | Indica que el programa `man` debe comportarse como el programa `apropos` (se explicará más adelante). |
| `-K` | Busca una cadena especificada en las páginas man. Por cada entrada encontrada se le pregunta al usuario si desea verla. |
| `-m` | Indica un conjunto alternativo de páginas man basado en el sistema especificado. |
| `-w` | Indica que ha de visualizarse el path de las páginas man. |

### Secciones del manual

| Sección | Contenido                                        |
| ------: | ------------------------------------------------ |
|     `1` | Comandos ejecutables y programas de usuario      |
|     `2` | Llamadas al sistema                              |
|     `3` | Funciones de biblioteca                          |
|     `4` | Archivos especiales y dispositivos               |
|     `5` | Formatos de archivos y archivos de configuración |
|     `6` | Juegos                                           |
|     `7` | Miscelánea y convenciones                        |
|     `8` | Comandos de administración del sistema           |


---

## Navegación
Tambien podemos usar *comandos basicos* como `ls` para ver el contenido de la ruta donde nos encontramos actualmente.
A este comando le podemos poner distintos `parametros/opciones` para ver mas informacion

| Opciones  | Funcion                                                      |
| --------- | -------------------------------------------------------------| 
| -a        | Muestra todos los archivos incluso los ocultos               | 
| -l        | Muestra mas informacion como tipo y permisos del archivo     |
| -h        | Muestra el tamaño de los archivos de una manera comoda       | 

O el comando `pwd` para ver la ruta donde nos encontramos y tambien tenemos el comando `cd` para movernos en niveles de directorios (avanzar o retroceder)

---

## Comandos basicos

### mkdir

Se utiliza para crear directorios. Se puede usar `-p` para crear un directorio y una cadena de subdirectorios

```
# mkdir directorio
```

```
# mkdir -p es/otro/directorio/lejano
# ls -ld es/otro/directorio/lejano/
drwxr-xr-x 2 root root 4096 Jun 12 02:57 es/otro/directorio/lejano/<
```

### cp

Se utiliza para copiar archivos/directorios

```
cp [ opciones ] archivo1 archivo2
cp [ opciones ] archivos directorio
```

### mv

Este comando se utiliza para mover o renombrar archivos y directorios.

```
mv [opciones] origen destino
```

### touch

Este comando cambia la fecha/hora de acceso o modificación de los archivos. Pero tambien se puede utilizar para crear archivos

```
touch [opciones] archivo
```

### rm

Este comando sirve para borrar tanto directorios como archivos.

```
rm [opciones] archivos
```

### rmdir

Este comando sirve para borrar directorios vacios.

```
rmdir [opciones] directori
```

---

## Comodines

Los comodines o metacaracteres nos van a servir para tomar acción sobre múltiples
archivos/directorios que contengan cierta cadena de caracteres en su nombre.

| Caracter  | Funcion                                                              |
| --------- | ---------------------------------------------------------------------|
| `*`       | Coincide con cualquier cadena de texto, incluso con la cadena vacía. | 
| `?`       | Coincide con un único carácter                                       |

---

## Compresion y archivado 

### Comando dd

dd permite copiar datos desde una entrada (if) hacia una salida (of), trabajando con bloques de información. 
Puede utilizarse para copiar archivos, crear imágenes, clonar dispositivos, escribir imágenes ISO sobre dispositivos de almacenamiento y realizar determinadas tareas de manipulación de datos.

```
dd [options] 
```

`if` => archivo Toma como entrada un archivo o dispositivo.

`of` => archivo Salida a un archivo en vez de la salida estándar.

`ibs` => n Leer de a "n" bytes.

`obs` => n Escribir de a n bytes

`conv` => list Convertir ciertos aspectos de lo que copio

### Comandos gzip / gunzip

Con estos comandos vamos a poder comprimir y descomprimir archivos

```
gzip [opciones] [archivo]
```

Algunas de las opciones mas usadas son:

`-d` => Para descomprimir un archivo (Equivale al comando gunzip).
`-1` => La compresión más rápida.
`-9` => La mejor compresión.

Hay otras tecnologias que realizan los mismo pero con distintos algoritmos como `bzip2` y `bunzip2` o `xz` y `unxz`

### Comando tar

Los comandos bzip, gzip y xz pueden comprimir un único archivo a la vez. El comando tar, en
cambio, sirve para poder agrupar archivos y directorios, y comprimirlos con esas
herramientas. El archivo resultante suele llamarse `tarball`.

**Opciones**

* `c` para crear.
* `x` para extraer.
* `v` modo verbose, muestra lo que se está haciendo.
* `f` define el archivo .tar.
* `z` comprime/descomprime con gzip.
* `j` comprime/descomprime con bzip2.
* `J` comprime/descomprime con xz.
* `t` muestra el contenido del tarball.

---

# Practica

Vamos a empezar realizando la siguiente estructura

```
lucascossia/
├── documentos/
│   ├── texto1.txt
│   └── texto2.txt
├── backups/
└── scripts/
```

Todo los directorios los podemos realizar con un solo comando

```
mkdir -p lucascossia/{documentos,backups,scripts}
```

Y despues creamos los archivos

```
touch lucascossia/documentos/texto1.txt
touch lucascossia/documentos/texto2.txt
```

O nos podemos mover al directorio `lucascossia/documentos/` con `cd` y despues simplemente `touch texto1.txt texto 2.txt`

Podemos consultar la informacion de algunos archivos para despues hacer un backup

```
ls -lah lucascossia/
stat lucascossia/documentos/texto1.txt
tar -czf lucascossia.tar.gz lucascossia/
tar -tzf lucascossia.tar.gz
```