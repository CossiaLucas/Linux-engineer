# Gestion de usuarios y grupos

Antes de pasar a explicar comandos para crear/gestionar usuarios vamos a explicar algunos conceptos importantes.

# Archivos de configuracion

* ## Archivo /etc/passwd

Contiene un listado de los usuarios locales. Cada archivo es una cuenta de usuario. Y veamos la descripción de cada campo.

| Campo | Descripcion  |
| --- | --- |
| `UserName` | Es el nombre de usuario | 
| `X` | Es comúnmente conocido como el campo de “password” y se encuentra directamente ligado a la contraseña del usuario. *|
| `UserID` | Identificador del Usuario |
| `GroupID` | Identificador que indica a cuál grupo pertenece el usuario. |
| `UserInfo` | Información adicional |
| `HomeDir`  | Home del Usuario, directorio de trabajo del usuario. |
| `Shell`  | Se puede asignar un shell distinto a cada usuario |

*
> a. La letra `x` indica que el password del usuario se encuentra cifrado y ligado directamente al archivo /etc/shadow 
> b. Si se encontrase este campo vacío, significa que el usuario no tiene contraseña. |

* ## Archivo /etc/shadow

Este fichero almacena las contraseñas cifradas de cada usuario del sistema. Estos datos se encuentran organizados línea a línea, de las cuales cada una corresponde a un usuario.