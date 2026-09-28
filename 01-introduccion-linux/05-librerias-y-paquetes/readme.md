# Librerias y gestion de paquetes

## **Librerías**

Una **librería** es una colección de código reutilizable, generalmente organizada en funciones, que puede ser utilizada por diferentes programas.

Cuando se desarrolla un programa, muchas de las funciones que necesita ya existen en librerías proporcionadas por el sistema u otros paquetes. 
Por ejemplo, puede necesitar funciones relacionadas con la gestión de memoria, archivos, red, entrada y salida, etc.

En lugar de implementar nuevamente estas funciones dentro de cada programa, los desarrolladores pueden utilizar librerías existentes.

Esto permite:

* Reutilizar código.
* Reducir el tamaño de los programas.
* Facilitar el mantenimiento.
* Permitir que varios programas compartan una misma librería.

## **Librerías estáticas y dinámicas**

### Librerías estáticas

Una librería estática se enlaza con el programa durante el proceso de compilación/enlazado. Parte del código de la librería necesario para el programa queda incorporado dentro del ejecutable.

Características:

* El ejecutable puede ser más grande.
* El programa contiene dentro de sí el código de las librerías utilizadas.
* No necesita disponer de esa librería compartida en tiempo de ejecución.
* Si la librería original cambia posteriormente, el programa ya compilado no incorpora automáticamente esos cambios.

Las librerías estáticas suelen utilizar archivos con extensión `.a`.

### Librerías dinámicas

Una librería dinámica se mantiene separada del ejecutable y se carga durante la ejecución del programa.

En Linux suelen utilizarse archivos con extensión `.so` (Shared Object).

Características:

* Varios programas pueden utilizar una misma librería.
* Los ejecutables suelen ser más pequeños.
* La librería debe estar disponible en el sistema en tiempo de ejecución.
* Una actualización de la librería puede afectar a varios programas que la utilizan, siempre que mantenga la compatibilidad necesaria.

El comando `ldd` permite ver las librerias comartidas que requiere un programa 

Tambien, estas dueles encontrarse en los directorios como:

* /lib
* /libx32
* /lib64

o tambien, en estos directorios en distribuciones de redhat:

* /usr/lib
* /usr/lib64

### **Comando `ldd`**

El comando `ldd` muestra las bibliotecas compartidas que necesita un programa para ejecutarse.

Por ejemplo:

``` bash
ldd /bin/ls
libselinux.so.1 => /lib64/libselinux.so.1
libc.so.6 => /lib64/libc.so.6
libpcre.so.1 => /lib64/libpcre.so.1
...
```

---

# **Paquetes DEB (Debian)**

Los paquetes `.deb` son el formato de paquetes utilizado por Debian y por distribuciones derivadas como Ubuntu.

Un paquete `.deb` contiene:

* Los archivos que serán instalados.
* Información de control y metadatos.
* Información relacionada con las dependencias.
* Scripts que pueden ejecutarse durante la instalación, configuración o eliminación del paquete.

El formato `.deb` es un archivo `ar` que contiene diferentes componentes, entre ellos archivos TAR con los datos y la información de control.

## Nomenclatura

``` text
<nombre>_<Nro.Version>-<NúmeroDeRevisión>_<arquitectura>.deb
```

## DPKG

Los paquetes en sistemas *Debian/Ubuntu* tienen la extensión `deb`, y son manejados por un programa llamado `dpkg`. Este es una herramienta para instalar, eliminar y manipular paquetes de Debian/Ubuntu. La herramienta principal de Debian para gestionar paquetes es apt-get

Los paquetes `deb` contienen tres *archivos*:

* `debian-binary` => Contiene el número de versión del paquete .deb.

* `control.tar.gz` => Contiene la información de control del paquete en una serie de ficheros de texto, por ejemplo, dependencias del paquete, prioridad, mantenedor, arquitectura, conflictos, versión, md5sum, etc.

* `data.tar` => Contiene todos los archivos que se instalarán, con sus rutas de destino.

### Ejemplos

Para instalar el programa `bzip2`

``` bash
dpkg -i bzip2_1.0.5-6_i386.deb
```

Para desinstalar el programa `quota`

``` bash
dpkg -r quota 
```

---

Tambien podemos obtener informacion mediante `dpkg`. Mediante los parametros:

| Parametro    | Funcion  |
| --- | --- |
| `-L` | Lista los archivos instalados por el paquete. | 
| `-s`  | Obtiene información del paquete, como el estado, versión, dependencias, etc. |

---

# Sistema APT

El sistema de gestión de paquetes **APT** (Advanced Packaging Tool), fue creado por el proyecto Debian y se utiliza para la instalación y eliminación de programas en sistemas GNU/Linux. APT fue rápidamente utilizado para funcionar con paquetes `.deb`.

## Comando `apt-get`

`apt-get` no trabaja directamente con los paquetes `.deb` como lo hace `dpkg`, sino que utiliza los nombres de los paquetes.

Por ejemplo, en `dpkg` utilizo: 

``` bash
dpkg -i bzip2_1.0.5-6_i386.deb
```

mientras que con `apt-get` utilizo:

``` bash
apt-get install bazip2
```

`apt-get` tiene una base de datos con información que le permite a la herramienta actualizar automáticamente paquetes y sus dependencias, como también instalar nuevos paquetes disponibles.

## Parametros frencuentes

| Parametro | Funcion  |
| --- | --- |
| `-d`  | Descarga los archivos, pero no los instala. | 
| `-s`  | No realiza ninguna acción, simula lo que hubiese ocurrido pero sin hacer cambios en el sistema. |
| `-y`  | Responde que sí (yes), a todas las preguntas que nos realiza la herramienta. |

## Comandos frencuentes

| Comando | Funcion  |
| --- | --- |
| `install` | Instala o actualiza uno o más paquetes. | 
| `remove`  | Remueve los paquetes seleccionados. |
| `update`  | Sincroniza el listado de paquetes disponibles en los repositorios |
| `upgrade` | Realiza una actualización de todos los paquetes. |
| `clean`   | Borra los paquetes de instalación descargados |

## Comando `apt-cache`

Esta herramienta se utiliza para consultar la caché local de la base de datos de paquetes de Debian.

### Comandos frencuentes

| Comando | Funcion  |
| --- | --- |
| `showpkg` | Muestra información acerca del paquete y sus dependencias entre otras cosas. | 
| `show`    | Muestra descripción acerca del paquete y paquetes sugeridos. |
| `search`  | Busca un paquete por su nombre o descripción. |

---

Tambien vale mencionar el mas usado, que es `apt`. Una herramienta mas sencilla, es equivalente a `apt-get` y `apt-search` combinadas.

Y `apt-file`, que no viene instalado de manera predeterminada, y sirve para buscar paquetes que contienen un determinado archivo.
