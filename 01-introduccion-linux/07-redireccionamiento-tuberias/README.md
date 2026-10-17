# **Redireccionamientos**

Antes de estudiar los redireccionamientos, es importante comprender cómo Linux administra los flujos de entrada y salida de los procesos.

Por defecto, los procesos disponen de tres descriptores de archivo estándar. Estos descriptores permiten recibir datos, mostrar resultados y comunicar errores.

## **Flujos estándar**

- **STDIN (0) — Standard Input:** entrada estándar. Es la fuente desde la que un proceso recibe datos.
- **STDOUT (1) — Standard Output:** salida estándar. Se utiliza para emitir los resultados normales de un comando.
- **STDERR (2) — Standard Error:** salida de error estándar. Se utiliza para emitir mensajes de error y diagnóstico.

En una terminal interactiva, STDIN normalmente está conectado al teclado, mientras que STDOUT y STDERR están conectados a la terminal. Estos flujos pueden redirigirse hacia archivos, dispositivos u otros procesos.

Es importante comprender que **STDOUT y STDERR son flujos independientes**. Esto permite guardar los resultados normales de una operación en un archivo y, al mismo tiempo, registrar sus errores en otro.

---

## **Operadores de redireccionamiento**

### **Salida estándar (STDOUT)**

La salida estándar contiene los resultados normales de un comando. De manera predeterminada, se muestra en la terminal.

El operador `>` permite redirigir STDOUT hacia un archivo.

```bash
comando > archivo.txt
```

Si el archivo no existe, se crea. Si ya existe, su contenido se trunca antes de escribir los nuevos datos.

Por otro lado, el operador `>>` agrega la salida al final del archivo, conservando el contenido existente.

### **Salida de error (STDERR)**

Los mensajes de error se envían normalmente a STDERR, cuyo descriptor es `2`.

Para redirigirlos se utilizan los operadores `2>` y `2>>`.

- `2>`: crea el archivo de destino si no existe y sobrescribe su contenido si ya existe.
- `2>>`: agrega los errores al final del archivo.

### **Entrada estándar (STDIN)**

La entrada estándar permite que un comando reciba datos desde una fuente determinada.

El operador `<` conecta el contenido de un archivo con STDIN del comando.

```bash
comando < archivo.txt
```

El comando procesa los datos recibidos sin necesidad de abrir el archivo por su cuenta.

La entrada estándar también puede proceder del teclado o de otro proceso mediante una tubería.

### **Redireccionamiento de ambos flujos**

Es posible redirigir STDOUT y STDERR hacia un mismo destino.

- `&>`: redirige STDOUT y STDERR al mismo archivo en Bash.
- `> archivo 2>&1`: redirige STDOUT al archivo y después conecta STDERR al destino actual de STDOUT.
- `2>&1`: duplica el descriptor 1 en el descriptor 2, haciendo que ambos apunten al mismo destino en ese momento.

El orden de los operadores importa, porque las redirecciones se procesan de izquierda a derecha.

También es posible descartar una salida utilizando `/dev/null`, un dispositivo especial que descarta los datos escritos en él.

```bash
comando > /dev/null 2>&1
```

Esta redirección descarta tanto STDOUT como STDERR.

---

# **Tuberías (Pipes)**

Las tuberías permiten conectar la salida estándar de un proceso con la entrada estándar de otro. Se representan mediante el operador `|`.

```bash
comando1 | comando2
```

Esto permite encadenar varias herramientas para construir operaciones de procesamiento de datos.

Por ejemplo, una salida puede ordenarse, filtrarse, contarse y finalmente guardarse en un archivo.

## **Comportamiento de las tuberías**

Por defecto, `|` conecta STDOUT del primer comando con STDIN del segundo. **STDERR no se transmite automáticamente por la tubería**.

Esto es importante al construir scripts de administración, porque el último comando puede finalizar correctamente aunque un comando anterior haya fallado.

---

# **Procesamiento de flujos de texto con filtros**

Los filtros son comandos que reciben datos, realizan una transformación y producen un resultado. Muchos pueden recibir información desde STDIN o desde uno o varios archivos y emitir sus resultados por STDOUT.

La combinación de filtros mediante tuberías permite construir operaciones complejas utilizando herramientas pequeñas y especializadas.

---

## **Comando `cat`**

`cat` permite concatenar archivos y enviar su contenido a STDOUT. Se utiliza para visualizar archivos de texto, combinar archivos y alimentar tuberías.

### Opciones

| Opción | Descripción |
|---|---|
| `-n` | Numera todas las líneas de salida. |
| `-b` | Numera únicamente las líneas que no están vacías. |
| `-s` | Reduce las secuencias de líneas vacías consecutivas a una sola. |
| `-A` | Muestra caracteres no imprimibles y hace visibles los finales de línea y tabuladores. |
| `-E` | Muestra `$` al final de cada línea. |
| `-T` | Muestra los tabuladores como `^I`. |

---

## **Comando `cut`**

`cut` extrae partes de cada línea de un archivo o de STDIN. Permite seleccionar campos delimitados, caracteres o bytes.

### Opciones

| Opción | Descripción |
|---|---|
| `-d` | Define el delimitador utilizado para separar campos. |
| `-f` | Selecciona uno o varios campos. |
| `-c` | Selecciona posiciones de caracteres. |
| `-b` | Selecciona posiciones de bytes. |
| `--complement` | Selecciona todo excepto los campos, caracteres o bytes indicados. |
| `--output-delimiter` | Define el delimitador que se utilizará entre los campos de salida. |

Cuando se seleccionan campos con `-f`, el delimitador predeterminado es el tabulador.

La numeración de campos y posiciones comienza en 1. Se pueden especificar rangos y listas de posiciones.

Es importante distinguir `-c` de `-b`: los caracteres y los bytes no siempre coinciden, especialmente al procesar texto codificado en UTF-8.

---

## **Comando `head`**

`head` muestra el comienzo de un archivo o flujo de datos. De manera predeterminada, presenta las primeras 10 líneas.

### Opciones

| Opción | Descripción |
|---|---|
| `-n N` | Muestra las primeras N líneas. |
| `-n -N` | Muestra todo excepto las últimas N líneas. |
| `-c N` | Muestra los primeros N bytes. |
| `-c -N` | Muestra todo excepto los últimos N bytes. |
| `-q` | Evita mostrar encabezados con nombres de archivo al procesar varios archivos. |
| `-v` | Muestra siempre los encabezados de los archivos. |

La opción `-c` trabaja con bytes, no con caracteres Unicode.

---

## **Comando `tail`**

`tail` muestra el final de un archivo o flujo. De manera predeterminada, presenta las últimas 10 líneas.

### Opciones

| Opción | Descripción |
|---|---|
| `-n N` | Muestra las últimas N líneas. |
| `-n +N` | Muestra desde la línea N hasta el final. |
| `-c N` | Muestra los últimos N bytes. |
| `-f` | Continúa mostrando los datos que se agregan al archivo. |
| `-F` | Intenta seguir el archivo aunque sea reemplazado o recreado. |
| `-q` | Oculta encabezados de nombres de archivo. |

`tail -f` es especialmente útil para supervisar registros (*logs*) que se actualizan continuamente. La opción `-F` resulta útil cuando los archivos de registro se rotan o reemplazan.

---

## **Comando `less`**

`less` es un paginador interactivo que permite consultar archivos extensos sin cargarlos como un documento editable.

Permite desplazarse hacia adelante y hacia atrás, buscar texto y navegar por el contenido.

### Teclas principales

| Tecla | Función |
|---|---|
| `Espacio` | Avanza una pantalla. |
| `b` | Retrocede una pantalla. |
| `Enter` | Avanza una línea. |
| `g` | Va al comienzo. |
| `G` | Va al final. |
| `/texto` | Busca hacia adelante. |
| `?texto` | Busca hacia atrás. |
| `n` | Repite la búsqueda en la misma dirección. |
| `N` | Repite la búsqueda en la dirección contraria. |
| `q` | Sale del paginador. |

`less` también puede recibir datos desde una tubería. A diferencia de un editor como `vi` o `vim`, su función principal es consultar y navegar por el contenido.

---

## **Comando `paste`**

`paste` combina líneas de uno o varios archivos colocándolas una al lado de la otra, separadas por tabuladores de manera predeterminada.

### Opciones

| Opción | Descripción |
|---|---|
| `-d` | Define el delimitador utilizado entre los campos. |
| `-s` | Procesa cada archivo de forma serial, colocando sus líneas en una misma línea de salida. |
| `-z` | Utiliza el carácter NUL como terminador de línea en lugar del salto de línea, si la implementación lo admite. |

A diferencia de `cat`, que concatena contenidos uno detrás de otro, `paste` combina líneas correspondientes en columnas.

---

## **Comando `sort`**

`sort` ordena las líneas de un archivo o flujo de texto. Por defecto, utiliza un orden lexicográfico según la configuración regional (*locale*) del sistema.

### Opciones

| Opción | Descripción |
|---|---|
| `-f` | Ignora las diferencias entre mayúsculas y minúsculas durante la comparación. |
| `-r` | Invierte el orden del resultado. |
| `-k` | Define la clave o campo utilizado para ordenar. |
| `-t` | Define el separador de campos. |
| `-n` | Ordena numéricamente. |
| `-h` | Ordena números con sufijos legibles por humanos, como `K`, `M` o `G`. |
| `-M` | Ordena por nombres abreviados de meses en inglés. |
| `-d` | Utiliza un orden de tipo diccionario, considerando principalmente letras, números y espacios en blanco. |
| `-u` | Elimina líneas duplicadas en el resultado ordenado. |
| `-V` | Realiza una ordenación de tipo versión, cuando está disponible. |
| `-o` | Especifica el archivo donde se guardará el resultado. |

La opción `-k` permite definir una clave de ordenación y, opcionalmente, los caracteres que delimitan su inicio y final. Combinada con `-t`, permite ordenar registros por una columna específica.

Es importante no confundir `-n` con el orden lexicográfico: sin ordenación numérica, los valores se comparan como texto y el resultado puede no coincidir con su valor matemático.

---

## **Comando `uniq`**

`uniq` identifica o elimina líneas consecutivas duplicadas. Por este motivo, suele utilizarse después de `sort` cuando se necesita detectar repeticiones en un archivo sin ordenar.

### Opciones

| Opción | Descripción |
|---|---|
| `-c` | Cuenta las repeticiones consecutivas y las muestra junto a cada línea. |
| `-d` | Muestra solamente las líneas duplicadas. |
| `-u` | Muestra solamente las líneas que no están duplicadas. |
| `-i` | Ignora diferencias entre mayúsculas y minúsculas. |
| `-f N` | Omite los primeros N campos al comparar. |
| `-s N` | Omite los primeros N caracteres al comparar. |

`uniq` no busca duplicados en todo el archivo de manera independiente: compara líneas consecutivas. Por eso, para contar correctamente las apariciones de cada usuario en un archivo de logins sin ordenar, normalmente se combina con `sort`.

---

## **Comando `wc`**

`wc` (*word count*) cuenta líneas, palabras, bytes o caracteres de archivos y flujos de texto.

### Opciones

| Opción | Descripción |
|---|---|
| `-l` | Cuenta líneas. |
| `-w` | Cuenta palabras. |
| `-c` | Cuenta bytes. |
| `-m` | Cuenta caracteres según la configuración regional. |
| `-L` | Muestra la longitud de la línea más larga. |

Sin opciones, normalmente muestra el número de líneas, palabras y bytes.

Es importante diferenciar bytes de caracteres: un carácter UTF-8 puede ocupar varios bytes.

---

## **Comando `split`**

`split` divide un archivo en varios archivos más pequeños. Es útil cuando se necesita distribuir información en partes manejables o procesar archivos de gran tamaño.

### Opciones

| Opción | Descripción |
|---|---|
| `-l N` | Divide el contenido en archivos de hasta N líneas cada uno. |
| `-b N` | Divide por tamaño, medido en bytes. |
| `-C N` | Intenta limitar el tamaño de cada archivo respetando los límites de línea. |
| `-d` | Utiliza sufijos numéricos en lugar de alfabéticos. |
| `-a N` | Define la longitud de los sufijos. |
| `-n N` | Divide el archivo en N partes, según el modo especificado. |

Los archivos generados utilizan un prefijo y sufijos que permiten distinguir las partes. Las opciones disponibles pueden variar según la implementación de `split`.

---

## **Comando `tac`**

`tac` muestra las líneas de un archivo en orden inverso, comenzando por la última y terminando por la primera.

Su nombre es `cat` escrito al revés.

### Opciones

| Opción | Descripción |
|---|---|
| `-b` | Coloca el separador antes de cada registro en lugar de después. |
| `-r` | Interpreta el separador como una expresión regular, si la implementación lo admite. |
| `-s` | Define el separador utilizado para identificar los registros. |

Por defecto, los registros se separan mediante saltos de línea. `tac` invierte el orden de las líneas, no el orden de los caracteres dentro de cada línea.

---

## **Comando `tr`**

`tr` transforma, elimina o comprime conjuntos de caracteres que recibe desde STDIN. No suele trabajar directamente con nombres de archivo como argumento: normalmente se utiliza mediante redireccionamientos o tuberías.

### Opciones

| Opción | Descripción |
|---|---|
| `-d` | Elimina los caracteres pertenecientes al conjunto indicado. |
| `-s` | Comprime secuencias de caracteres repetidos del conjunto especificado. |
| `-c` | Complementa el conjunto de caracteres indicado. |
| `-t` | Trunca el primer conjunto para que tenga la longitud del segundo. |

`tr` trabaja con conjuntos de caracteres y es especialmente útil para transformaciones sencillas, como convertir letras minúsculas en mayúsculas o reemplazar determinados caracteres.

---

## **Comando `tee`**

`tee` lee datos desde STDIN y los envía simultáneamente a STDOUT y a uno o varios archivos.

Permite conservar el resultado de una tubería en un archivo sin impedir que los datos continúen hacia el siguiente comando.

### Opciones

| Opción | Descripción |
|---|---|
| `-a` | Agrega los datos al final de los archivos en lugar de sobrescribirlos. |
| `-i` | Ignora determinadas señales de interrupción, según la implementación. |

`tee` es especialmente útil para registrar resultados durante una secuencia de procesamiento.

---

## **Comando `grep`**

`grep` busca líneas que coincidan con un patrón de texto o una expresión regular. Puede procesar archivos directamente o recibir datos desde STDIN.

### Opciones

| Opción | Descripción |
|---|---|
| `-i` | Ignora diferencias entre mayúsculas y minúsculas. |
| `-v` | Muestra las líneas que no coinciden con el patrón. |
| `-n` | Muestra el número de línea. |
| `-c` | Cuenta las líneas que coinciden con el patrón. |
| `-r` | Busca recursivamente en directorios. |
| `-E` | Interpreta el patrón como una expresión regular extendida. |
| `-F` | Interpreta el patrón como texto literal, no como expresión regular. |
| `-l` | Muestra los nombres de los archivos que contienen coincidencias. |
| `-w` | Busca coincidencias como palabras completas. |

`grep` es una de las herramientas más utilizadas para filtrar registros de sistema, resultados de comandos y archivos de configuración.

---

# **Verificación de integridad mediante sumas de comprobación**

## **Comando `md5sum`**

`md5sum` calcula o verifica una suma de comprobación (*checksum*) mediante el algoritmo MD5.

Una suma de comprobación permite comparar el contenido de archivos: si el contenido cambia, normalmente también cambia su hash.

### Opciones

| Opción | Descripción |
|---|---|
| `-c` | Comprueba los hashes definidos en un archivo de sumas de comprobación. |
| `-b` | Lee o procesa los archivos en modo binario; la diferencia práctica depende del sistema. |
| `-t` | Procesa los archivos en modo texto; en muchos sistemas Unix la diferencia con `-b` es inexistente. |

**Limitación de seguridad:** MD5 presenta vulnerabilidades conocidas frente a colisiones. No debe utilizarse para garantizar autenticidad ni seguridad criptográfica. Para comprobar integridad con un algoritmo más robusto, suele preferirse `sha256sum`.

Además, que dos archivos tengan el mismo hash MD5 no constituye una prueba matemática de que sean idénticos.

---

# **Lectura de archivos comprimidos**

Linux dispone de herramientas que permiten consultar el contenido de archivos comprimidos sin tener que descomprimirlos previamente en un archivo de destino.

Estas herramientas escriben el contenido descomprimido en STDOUT, por lo que pueden utilizarse con tuberías y otros filtros.

## **Comando `zcat`**

`zcat` muestra el contenido descomprimido de archivos comprimidos mediante gzip, normalmente con extensión `.gz`.

Se utiliza para consultar registros comprimidos sin generar una copia descomprimida en el disco.

La compatibilidad exacta de `zcat` con archivos no comprimidos puede variar entre implementaciones.

## **Comando `bzcat`**

`bzcat` descomprime y muestra por STDOUT el contenido de archivos comprimidos con bzip2, habitualmente identificados con la extensión `.bz2`.

Permite procesar el contenido directamente con otros comandos sin guardar una versión descomprimida intermedia.

## **Comando `xzcat`**

`xzcat` muestra el contenido descomprimido de archivos comprimidos con xz, generalmente con extensión `.xz`.

Al igual que `zcat` y `bzcat`, puede integrarse en tuberías de procesamiento de texto.

---

# **Resumen**

Los redireccionamientos, las tuberías y los filtros forman parte de las herramientas esenciales de la administración de sistemas Linux.

- Los **redireccionamientos** permiten cambiar el origen o destino de los flujos estándar.
- Las **tuberías** conectan la salida de un proceso con la entrada de otro.
- Los **filtros** permiten seleccionar, ordenar, contar, transformar y analizar datos.
- Las herramientas de **verificación de integridad** permiten detectar cambios en el contenido de archivos.
- Los comandos para archivos comprimidos permiten procesar información sin generar archivos temporales innecesarios.

Dominar estas herramientas permite automatizar tareas, analizar registros, administrar archivos de configuración y construir scripts de shell más eficientes.