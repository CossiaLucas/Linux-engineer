# **Gestión de usuarios y grupos**

La gestión de usuarios y grupos es una de las tareas fundamentales de la administración de sistemas Linux.

Los usuarios permiten identificar y controlar quién puede acceder al sistema, mientras que los grupos permiten organizar permisos y recursos de forma colectiva.

En Linux, prácticamente todas las operaciones sobre archivos y procesos se realizan teniendo en cuenta la identidad del usuario y los grupos a los que pertenece.

Antes de estudiar los comandos de creación y administración, es importante comprender cómo Linux almacena esta información.

---

# **Archivos de configuración**

Los principales archivos involucrados en la administración local de usuarios y grupos son:

- `/etc/passwd`
- `/etc/shadow`
- `/etc/group`
- `/etc/gshadow`
- `/etc/login.defs`
- `/etc/skel`

> Estos archivos corresponden a cuentas **locales**. En sistemas administrados mediante LDAP, Active Directory, NIS u otros mecanismos, la información de usuarios también puede provenir de fuentes externas.

---

## **Archivo `/etc/passwd`**

El archivo `/etc/passwd` contiene información básica sobre las cuentas de usuario locales del sistema.

Cada línea representa una cuenta y está formada por **7 campos separados por `:`**.

Podemos visualizarlo con:

```bash
cat /etc/passwd
    lucacoss:x:1000:1000:Lucas Cossia:/home/lucacoss:/bin/bash
```

La estructura es:

```text
usuario:x:UID:GID:GECOS:home:shell
```

| Campo | Descripción |
|---|---|
| `usuario` | Nombre de la cuenta. |
| `x` | Indica que el hash de la contraseña se almacena en `/etc/shadow`. |
| `UID` | Identificador numérico del usuario. |
| `GID` | Identificador del grupo primario del usuario. |
| `GECOS` | Información descriptiva de la cuenta, como nombre completo o comentario. |
| `home` | Directorio personal del usuario. |
| `shell` | Shell asignado al usuario para iniciar sesión. |

### **UID — User ID**

El **UID (User ID)** es el identificador numérico que utiliza Linux internamente para identificar al usuario.

Por ejemplo:

```text
lucacoss:x:1000:1000:Lucas Cossia:/home/lucacoss:/bin/bash
        ↑
       UID
```

Aunque nosotros trabajemos normalmente con el nombre `lucacoss`, el kernel y muchos componentes del sistema utilizan el UID.

Podemos consultar nuestro UID con:

```bash
id -u
```

O consultar toda la información:

```bash
id lucacoss
    uid=1000(lucacoss) gid=1000(lucacoss) groups=1000(lucacoss),27(sudo)
```

> En muchas distribuciones modernas, las cuentas de usuarios normales comienzan a partir de UID `1000`. Este valor depende de la distribución y de su configuración; no lo podemos asumir como una regla universal.

### **GID — Group ID**

El GID identifica numéricamente al **grupo primario** asociado a la cuenta.

---

## **El campo `x` y `/etc/shadow`**

En sistemas Linux modernos es habitual encontrar:

```text
lucacoss:x:1000:1000:Lucas Cossia:/home/lucacoss:/bin/bash
```

La `x` **no representa la contraseña cifrada**.

Indica que la información relacionada con la contraseña se almacena en:

```text
/etc/shadow
```

Esto permite separar la información general de las cuentas de la información sensible relacionada con la autenticación.

Por ejemplo:

```bash
ls -l /etc/passwd /etc/shadow
    -rw-r--r-- 1 root root   ... /etc/passwd
    -rw-r----- 1 root shadow ... /etc/shadow
```

El acceso a `/etc/shadow` está mucho más restringido porque contiene información sensible de autenticación.

> Es más correcto hablar de **hash de contraseña** que de contraseña “cifrada”. Un hash es una transformación unidireccional diseñada para no permitir recuperar directamente la contraseña original.

---

# **Archivo `/etc/shadow`**

`/etc/shadow` almacena los hashes de las contraseñas y diferentes parámetros relacionados con la expiración de las credenciales.

Cada línea corresponde a una cuenta.

Podemos observar su contenido con:

```bash
sudo cat /etc/shadow
```

Una línea simplificada puede tener esta estructura:

```text
usuario:$y$...:19750:0:99999:7:::
```

Los campos están separados por `:`.

| Campo | Descripción |
|---|---|
| `usuario` | Nombre de la cuenta. |
| `password` | Hash de la contraseña o información de bloqueo. |
| `lastchange` | Fecha del último cambio de contraseña expresada en días desde el 01/01/1970. |
| `mindays` | Cantidad mínima de días antes de poder cambiar nuevamente la contraseña. |
| `maxdays` | Cantidad máxima de días de validez de la contraseña. |
| `warndays` | Días de anticipación para advertir sobre la expiración. |
| `inactivedays` | Días posteriores a la expiración antes de deshabilitar la cuenta. |
| `expiredate` | Fecha de expiración de la cuenta. |
| `reserved` | Campo reservado. |

Podemos consultar estos parámetros de manera más amigable mediante:

```bash
sudo chage -l lucacoss
```

---

# **Archivo `/etc/group`**

`/etc/group` contiene información sobre los grupos locales del sistema.

Cada línea representa **un grupo**, no un usuario.

Ejemplo:

```text
sudo:x:27:lucacoss
```

La estructura es:

```text
grupo:GID:usuarios
```

En realidad, `/etc/group` posee cuatro campos:

```text
grupo:password:GID:usuarios
```

| Campo | Descripción |
|---|---|
| `grupo` | Nombre del grupo. |
| `password` | Campo histórico relacionado con contraseñas de grupo; normalmente se utiliza `/etc/gshadow`. |
| `GID` | Identificador numérico del grupo. |
| `usuarios` | Lista de usuarios que tienen este grupo como grupo secundario. |

Por ejemplo:

```text
syslab:x:1002:devuser,labuser
```

Significa que:

- el grupo se llama `syslab`;
- su GID es `1002`;
- `devuser` y `labuser` pertenecen a él como miembros secundarios.

> El grupo primario de un usuario se determina mediante el GID almacenado en `/etc/passwd`. Por eso un usuario puede tener un grupo primario y, además, múltiples grupos secundarios.

---

# **Archivo `/etc/gshadow`**

`/etc/gshadow` contiene información sensible relacionada con los grupos, de manera similar a cómo `/etc/shadow` contiene información sensible de las cuentas.

Puede consultarse mediante:

```bash
sudo cat /etc/gshadow
```

Su acceso está restringido.

En la administración cotidiana normalmente no es necesario modificarlo directamente. Es preferible utilizar herramientas como:

```bash
groupadd
groupmod
groupdel
gpasswd
usermod
```

---

# **Archivo `/etc/login.defs`**

`/etc/login.defs` contiene parámetros utilizados por diferentes herramientas de administración de cuentas, especialmente para establecer valores predeterminados relacionados con la creación y expiración de usuarios.

Podemos consultarlo con:

```bash
cat /etc/login.defs
```

Por ejemplo:

```bash
grep -E '^(UID_MIN|UID_MAX|GID_MIN|GID_MAX|PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE)' /etc/login.defs
```

Entre los parámetros que podemos encontrar están:

- rangos de UID;
- rangos de GID;
- valores predeterminados relacionados con expiración de contraseñas;
- configuración utilizada por herramientas de creación de cuentas.

> Los valores efectivos pueden depender también de la distribución y de las herramientas instaladas. Por eso conviene consultar siempre la documentación de la distribución utilizada.

---

# **Directorio `/etc/skel`**

`/etc/skel` contiene archivos y directorios que pueden utilizarse como plantilla al crear el directorio personal de un nuevo usuario.

Podemos observarlo con:

```bash
ls -la /etc/skel
```

Por ejemplo:

```text
.bash_logout
.bashrc
.profile
```

Cuando se crea un usuario con:

```bash
sudo useradd -m labuser
```

la opción `-m` solicita la creación del directorio personal y normalmente se copian allí los archivos de `/etc/skel`.

Podemos comprobarlo:

```bash
ls -la /home/labuser
```

Esto permite que los nuevos usuarios comiencen con una configuración inicial de shell.

---

# **Gestión de usuarios**

Las cuentas locales están representadas principalmente mediante:

```text
/etc/passwd
/etc/shadow
```

Cuando se crea un usuario, Linux debe establecer diferentes atributos:

- nombre de usuario;
- UID;
- grupo primario;
- grupos secundarios;
- directorio personal;
- shell;
- políticas de expiración;
- información de autenticación.

Para administrar estas cuentas existen diferentes herramientas.

---

# **Comando `useradd`**

`useradd` permite crear una nueva cuenta de usuario.

Sintaxis:

```bash
useradd [opciones] nombreDelUsuario
```

Por ejemplo:

```bash
sudo useradd -m -s /bin/bash labuser
```

Esto crea:

- el usuario `labuser`;
- su directorio `/home/labuser`;
- su shell `/bin/bash`.

Después debemos establecer la contraseña:

```bash
sudo passwd labuser
```

Podemos comprobar la cuenta:

```bash
id labuser
    uid=1001(labuser) gid=1001(labuser) groups=1001(labuser)
```

También podemos consultar su entrada en `/etc/passwd`:

```bash
getent passwd labuser
    labuser:x:1001:1001::/home/labuser:/bin/bash
```

## **Opciones importantes de `useradd`**

| Opción | Descripción |
|---|---|
| `-b` | Define el directorio base utilizado para los directorios personales. |
| `-d` | Especifica el directorio personal del usuario. |
| `-e` | Define la fecha de expiración de la cuenta. |
| `-f` | Define los días posteriores a la expiración de la contraseña antes de deshabilitar la cuenta. |
| `-g` | Define el grupo primario. |
| `-G` | Define grupos secundarios. |
| `-u` | Especifica manualmente el UID. |
| `-m` | Crea el directorio personal. |
| `-s` | Define el shell de inicio de sesión. |
| `-c` | Define información descriptiva/comentario de la cuenta. |

### **Crear un usuario con grupo secundario**

Por ejemplo:

```bash
sudo groupadd syslab
sudo useradd -m -s /bin/bash devuser
sudo usermod -aG syslab devuser
```

Podemos verificar:

```bash
id devuser
    uid=1002(devuser) gid=1003(devuser) groups=1003(devuser),1002(syslab)
```

---

# **Grupo primario y grupos secundarios**

Esta diferencia es fundamental para administrar permisos.

Supongamos:

```text
uid=1002(devuser) gid=1003(devuser) groups=1003(devuser),1002(syslab)
```

En este caso:

- `devuser` → usuario;
- GID `1003` → grupo primario;
- `syslab` → grupo secundario.

Cuando un usuario crea un archivo, el grupo asociado al archivo normalmente será su grupo primario:

```bash
touch archivo.txt
ls -l archivo.txt
    -rw-r--r-- 1 devuser devuser 0 archivo.txt
```

Si necesitamos trabajar con archivos compartidos entre varios usuarios, podemos utilizar un grupo común:

```bash
sudo groupadd syslab
sudo usermod -aG syslab devuser
sudo usermod -aG syslab labuser
```

Esto permite utilizar los permisos del grupo para controlar el acceso a recursos compartidos.

---

# **Comando `id`**

`id` muestra información sobre la identidad de un usuario.

---

# **Comando `usermod`**

`usermod` permite modificar los atributos de una cuenta existente.

Sintaxis general:

```bash
usermod [opciones] nombreDelUsuario
```

Algunas opciones importantes:

| Opción | Descripción |
|---|---|
| `-c` | Modifica el comentario de la cuenta. |
| `-d` | Cambia el directorio personal. |
| `-m` | Mueve el contenido del home al nuevo directorio utilizado junto con `-d`. |
| `-g` | Cambia el grupo primario. |
| `-G` | Define los grupos secundarios. |
| `-a` | Agrega grupos a los existentes cuando se utiliza junto con `-G`. |
| `-s` | Cambia el shell. |
| `-u` | Cambia el UID. |
| `-L` | Bloquea la cuenta. |
| `-U` | Desbloquea la cuenta. |
| `-e` | Define la fecha de expiración de la cuenta. |

### **Agregar un usuario a un grupo**

Este es un comando muy importante:

```bash
sudo usermod -aG syslab devuser
```

> Es importante utilizar `-aG` y no solamente `-G`. Si utilizamos `-G` sin `-a`, podemos reemplazar la lista actual de grupos secundarios del usuario.

El usuario normalmente deberá iniciar una nueva sesión para que los cambios de grupos se reflejen completamente en su entorno.

---

# **Comando `userdel`**

`userdel` elimina una cuenta de usuario.

Sintaxis:

```bash
sudo userdel [opciones] nombreDelUsuario
```

Esto elimina la cuenta, pero **no necesariamente elimina su directorio personal ni todos sus archivos**.

La opción `-r` elimina el directorio personal y el contenido que se encuentre allí, además de la cuenta.

> Antes de utilizar `-r`, debemos verificar que no existan datos que necesitemos conservar.

La opcion `-f` fuerza determinadas operaciones de eliminación incluso en situaciones en las que normalmente `userdel` no las realizaría.

> Es una opción potencialmente peligrosa y no debería utilizarse como método habitual para eliminar usuarios.

---

# **Comando `passwd`**

`passwd` permite administrar las contraseñas de las cuentas.

Para cambiar la contraseña de otro usuario, normalmente necesitamos privilegios administrativos:

```bash
sudo passwd labuser
```

También podemos bloquear y desbloquear una cuenta:

- `-l` → bloquea la autenticación mediante contraseña.
- `-u` → desbloquea la contraseña.

---

# **Consultar usuarios con `getent`**

`getent` es especialmente útil porque permite consultar las bases de datos de cuentas utilizando el mecanismo de resolución configurado en el sistema.

Por ejemplo:

```bash
getent passwd labuser
    labuser:x:1001:1001::/home/labuser:/bin/bash
```

También podemos consultar grupos:

```bash
getent group syslab
```

Ejemplo:

```text
syslab:x:1002:devuser,labuser
```

Esto es preferible en muchos casos a consultar directamente `/etc/passwd` o `/etc/group`, especialmente en sistemas que utilizan fuentes externas de autenticación.

---

# **Gestión de grupos**

Los grupos permiten asignar permisos a múltiples usuarios de manera conjunta.

En lugar de otorgar permisos individualmente, podemos crear un grupo:

```text
developers
```

y agregar los usuarios correspondientes:

```bash
sudo usermod -aG developers usuario1
sudo usermod -aG developers usuario2
sudo usermod -aG developers usuario3
```

Después podemos asignar permisos al grupo sobre un determinado recurso.

---

# **Comando `groupadd`**

`groupadd` permite crear nuevos grupos.

Sintaxis:

```bash
sudo groupadd [opciones] nombreDelGrupo
```

## **Opciones importantes**

| Opción | Descripción |
|---|---|
| `-g` | Define manualmente el GID. |
| `-r` | Crea un grupo del sistema. |
| `-f` | Si el grupo ya existe, termina sin generar un error; no significa que vaya a duplicar el grupo. |
| `-o` | Permite utilizar un GID que ya está asignado a otro grupo. Debe utilizarse con mucho cuidado. |

### **Grupos normales y grupos del sistema**

Los grupos del sistema suelen utilizarse para servicios y componentes del sistema.

Por ejemplo:

```bash
sudo groupadd -r webservice
```

El rango numérico utilizado para distinguir grupos de sistema y grupos normales **depende de la distribución y su configuración**.

No es correcto asumir universalmente que todos los grupos con GID menor a `500` son grupos de sistema.

En sistemas Linux modernos es frecuente encontrar rangos diferentes, por ejemplo alrededor de `100–999` para cuentas/grupos del sistema y desde `1000` para cuentas normales, pero debemos consultar la configuración concreta del sistema.

---

# **Comando `groupmod`**

`groupmod` permite modificar un grupo existente.

Sintaxis:

```bash
sudo groupmod [opciones] nombreDelGrupo
```

Opciones importantes:

| Opción | Descripción |
|---|---|
| `-g` | Cambia el GID del grupo. |
| `-n` | Cambia el nombre del grupo. |

> Cambiar un GID puede requerir actualizar la propiedad de archivos existentes que todavía tengan asociado el GID anterior.

---

# **Comando `groupdel`**

Elimina un grupo:

```bash
sudo groupdel developers
```

> No debemos eliminar un grupo que sea el grupo primario de un usuario. Primero debemos cambiar el grupo primario de esos usuarios.

---

# **Comando `gpasswd`**

`gpasswd` permite administrar miembros y administradores de grupos.

Para una administración habitual, `usermod -aG` es una herramienta mas utilizada.

---

# **Comando `groups`**

`groups` muestra los grupos a los que pertenece un usuario.

```bash
groups lucacoss
    lucacoss : lucacoss sudo
```

Si no especificamos un usuario:

```bash
groups
```

se muestran los grupos de la cuenta que ejecuta el comando.

---

# **Comando `grpck`**

`grpck` verifica la integridad de los archivos relacionados con los grupos locales.

Por ejemplo:

```bash
sudo grpck
```

La herramienta puede detectar inconsistencias en:

```text
/etc/group
/etc/gshadow
```

No se utiliza normalmente pasando el nombre de un grupo, sino para verificar la integridad de las bases de datos de grupos.

---

# **Políticas de usuarios y contraseñas**

Linux permite definir políticas relacionadas con la expiración de cuentas y contraseñas.

Estas políticas son importantes en entornos empresariales, donde puede ser necesario establecer:

- duración máxima de una contraseña;
- período mínimo entre cambios;
- advertencias antes de la expiración;
- fecha de expiración de una cuenta;
- bloqueo posterior a la expiración.

---

# **Comando `chage`**

`chage` permite consultar y modificar las políticas de expiración de contraseñas de un usuario.

Sintaxis:

```bash
sudo chage [opciones] nombreDelUsuario
```

Para consultar la configuración actual:

```bash
sudo chage -l labuser
```

Podemos obtener información similar a:

```text
Last password change                                    : Oct 05, 2026
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

## **Opciones importantes**

| Opción | Descripción |
|---|---|
| `-d` | Establece la fecha del último cambio de contraseña. |
| `-E` | Establece la fecha de expiración de la cuenta. |
| `-I` | Define los días de inactividad después de que expire una contraseña antes de bloquear la cuenta. |
| `-m` | Define el número mínimo de días entre cambios de contraseña. |
| `-M` | Define el número máximo de días de validez de la contraseña. |
| `-W` | Define cuántos días antes de la expiración se mostrará una advertencia. |
| `-l` | Muestra la información de expiración de la cuenta. |

### **Ejemplo**

Podemos establecer que una contraseña tenga una duración máxima de 90 días:

```bash
sudo chage -M 90 labuser
```

Que el usuario reciba una advertencia 7 días antes:

```bash
sudo chage -W 7 labuser
```

Y que deba esperar al menos 1 día entre cambios:

```bash
sudo chage -m 1 labuser
```

Podemos verificar:

```bash
sudo chage -l labuser
```

---

# **Comando `su`**

`su` permite cambiar de usuario dentro de una sesión existente.

Por ejemplo:

```bash
su labuser
```

El sistema solicitará la contraseña de `labuser` y, si es correcta, inicia un shell con esa identidad.

Para cambiar a `root`:

```bash
su -
```

---

## **Opciones importantes de `su`**

| Opción | Descripción |
|---|---|
| `-c` | Ejecuta un comando utilizando la identidad del usuario indicado. |
| `-l` | Inicia un login shell. |
| `-m` | Intenta preservar el entorno del usuario actual. |

### **`su` y `sudo`**

Es importante diferenciar ambos comandos.

`su` cambia la identidad efectiva del usuario y normalmente solicita la contraseña del usuario objetivo.

`sudo`, en cambio, permite ejecutar un comando con privilegios de otro usuario, normalmente `root`, de acuerdo con las reglas configuradas en `/etc/sudoers` y archivos relacionados.

Por ejemplo:

```bash
sudo systemctl restart ssh
```

---

# **Comandos útiles para consultar cuentas**

Además de las herramientas anteriores, existen otros comandos muy útiles para la administración diaria.

## **`whoami`**

Muestra el usuario con el que estamos ejecutando la sesión:

---

## **`who`**

Muestra los usuarios actualmente conectados:

```bash
who
```

---

## **`w`**

Muestra usuarios conectados y cierta información sobre sus sesiones y actividad:

```bash
w
```

---

## **`last`**

Consulta el historial de inicios de sesión registrado por el sistema:

```bash
last
```

---

# **Ejemplo completo: creación y administración de un usuario**

Podemos integrar varios de los comandos anteriores en un pequeño ejercicio práctico.

### **1. Crear un grupo**

```bash
sudo groupadd syslab
```

### **2. Crear un usuario**

```bash
sudo useradd -m -s /bin/bash labuser
```

### **3. Establecer contraseña**

```bash
sudo passwd labuser
```

### **4. Agregar el usuario al grupo**

```bash
sudo usermod -aG syslab labuser
```

### **5. Verificar la identidad**

```bash
id labuser
    uid=1001(labuser) gid=1001(labuser) groups=1001(labuser),1002(syslab)
```

### **6. Verificar la información de la cuenta**

```bash
getent passwd labuser
    labuser:x:1001:1001::/home/labuser:/bin/bash
```

### **7. Verificar el grupo**

```bash
getent group syslab
    syslab:x:1002:labuser
```

### **8. Verificar el directorio personal**

```bash
ls -ld /home/labuser
    drwxr-x--- 2 labuser labuser 4096 Oct 05 20:00 /home/labuser
```

### **9. Consultar las políticas de contraseña**

```bash
sudo chage -l labuser
```

### **10. Probar el cambio de usuario**

```bash
su - labuser
```

Y finalmente:

```bash
whoami
    labuser
```
