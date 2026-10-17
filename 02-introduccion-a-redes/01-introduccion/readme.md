# Redes de datos

Una red de datos es un conjunto de equipos informáticos, de comunicaciones, software y medios de transporte que permiten el **envío y recepción de información** utilizando diversas tecnologías. 

Este modelo, en el caso de las redes de datos, tiene sus objetivos bien definidos: 

* Compartir recursos e información a distancia. 
* Asegurar la confiabilidad y disponibilidad de la información. 
* Acelerar la velocidad de transmisión de datos. 
* Aumentar la eficiencia y reducir los costos. 

Elementos de la comunicación en redes: 

| `Elemento` | `Descripcion` |
|---|---|
| **Mensaje** | Información que se va transportar por una red |
| **Protocolo** | Conjunto de reglas que especifican la manera en cómo los mensajes se envían a través de una red  |
| **Emisor** | Elemento que envía el mensaje |
| **Receptor** | Elemento que recibe el mensaje  |
| **Medio** | Por donde se transporta el mensaje  |

## Ejemplos de mensajes

* **Programas de mensajeria** (Como whastapp)
* **Navegador web** (Donde se envia una peticion a un servidor web)
* **Servicios de streaming**

## Ejemplos de protocolos

* El Protocolo de transferencia de hipertexto (**HTTP**)
* Protocolo para transferencia simple de correo (**SMTP**)

## Modelos de capas

El modelo de interconexión de sistemas abiertos, conocido como modelo `OSI`, es un modelo de referencia para los **protocolos de la red**. Es un estándar que tiene por objetivo conseguir interconectar sistemas de procedencia distinta para que estos pudieran intercambiar información sin ningún tipo de impedimentos

[Imagen representatica de los modelos](./images/img01.png)

El modelo OSI se usa como referencia 

[Imagen de las capas OSI](./images/img02.png)

1. `Capa 7:` De la aplicación necesaria para abrir el mensaje 

2. `Capa 6:` Encriptación e interpretación del mensaje 

3. `Capa 5:` Información de sesión en la aplicación 

4. `Capa 4:` Protocolo TCP y UDP 

5. `Capa 3:` Direccionamiento lógico, donde se encuentra la ip 

6. `Capa 2:` Direccionamiento físico, donde se encuentra la mac 

7. `Capa 1:` Transmisión binaria, mediante pulsos eléctricos 

# Dispositivos

## Hosts

Son aquellos equipos conectados a una red que proveen y utilizan servicios de ella. El término hosts agrupa varios tipos de dispositivos: 

### Dispositivos de usuario final

Son los dispositivos que hacen uso de la red, son aquellos que ejecutan programas (software) que generan e interpretan la información que se envía o recibe.

* Computadoras
* Celulares
* Impresoras

### Servidores

Son los hosts que ponen a disposición de los usuarios los distintos servicios.

### Dispositivos de red 

Los dispositivos de red conectan dispositivos de usuario y servidores, agrupándolos en segmentos de pocos o muchos otros dispositivos de usuario.
Dentro de una red cada dispositivo tiene un rol, una función específica dentro de todo el proceso.

La interfaz de red o placa de red o sencillamente NIC, es el dispositivo que utilizan los dispositivos de usuario final y servidores para volcar y recibir información hacia y desde la red de datos

* `Inalámbrica`, utiliza ondas de radio, por ejemplo WiFi, Bluetooth.
* `Cableada de cobre`, utiliza cables que transportan pulsos eléctricos, por ejemplo el cable UTP o el coaxial.
* `Ópticas`, utilizan señales lumínicas que pueden ser transportadas por cables de fibra óptica.

### SWITCH 

Las redes pueden tener tamaños variables, pero para definir una red necesitamos al menos 2 dispositivos que se conecten entre sí. A este tipo de redes se las denomina `punto a punto`, es un tipo de red que permite la conexión entre dos nodos.

Es en este contexto donde el `switch` permite la incorporación de nuevos dispositivos extendiendo la red.

### Router

Este dispositivo se encarga de transportar los mensajes entre redes distintas, tanto a segmentos `físicos` como a `redes lógicas`.

### Medio

Lo entendemos como el medio físico por donde se transporta la información. Algunos ejemplos son:

* Cable UTP
* Cable RJ45
* Conector BNC
* Cable Coaxial
* Cable de fibra optica

### Otros dispositivos

`Repetidor`: Aumenta el alcance de una conexión física, recibiendo las señales y retransmitiendolas, para evitar su degradación, a través del medio de transmisión, lográndose un alcance mayor

`Concentrador`: Funciona como un repetidor pero permite la interconexión de múltiples nodos. (Tambien se llama `concentrador`)


