# Cisco Packet Tracer: diseño y simulación de una red LAN redundante

## 1. Introducción a Cisco Packet Tracer

**Cisco Packet Tracer** es una herramienta de simulación de redes desarrollada por Cisco. Permite diseñar topologías, incorporar dispositivos, establecer conexiones, configurar parámetros y observar el comportamiento de una infraestructura de red de manera virtual.

Se utiliza principalmente con fines educativos y de práctica, ya que permite experimentar con routers, switches, computadoras, servidores y otros dispositivos sin necesidad de disponer de todo el equipamiento físico.

Entre sus principales funcionalidades se encuentran:

- Diseñar topologías físicas y lógicas.
- Incorporar y conectar routers, switches, hosts y servidores.
- Configurar direcciones IP, máscaras, puertas de enlace y VLAN.
- Practicar la configuración de routers y switches mediante interfaces gráficas o comandos de consola, según el dispositivo.
- Simular el envío de paquetes y observar cómo atraviesan la red.
- Analizar eventos y protocolos mediante el modo **Simulation**.
- Detectar errores de conectividad y comprobar el resultado de los cambios de configuración.

> Packet Tracer es un simulador educativo. Sus dispositivos representan determinados comportamientos de equipos reales, pero no reemplazan por completo un laboratorio físico ni implementan todas las características de cada equipo Cisco.

## 2. Interfaz de Packet Tracer

![Interfaz de Cisco Packet Tracer](./images/img03.png)

Al iniciar Packet Tracer se presenta un entorno de trabajo que permite crear, configurar y simular redes. La distribución exacta puede variar según la versión, pero sus principales áreas son las siguientes.

### 2.1. Barra de menú y herramientas

Ubicada en la parte superior, contiene opciones para administrar el proyecto y operar sobre el espacio de trabajo. Entre las funciones habituales se encuentran:

- Crear, abrir y guardar proyectos.
- Copiar, pegar y eliminar elementos.
- Seleccionar, mover y organizar dispositivos.
- Acercar o alejar la vista.
- Incorporar anotaciones y elementos gráficos.
- Acceder a herramientas de configuración y visualización.

### 2.2. Área de trabajo lógica (*Logical Workspace*)

Es la zona central, generalmente representada como un espacio en blanco, donde se construye el diagrama de la red.

Permite incorporar equipos, ubicarlos, conectarlos, agregar etiquetas y visualizar el estado de los enlaces. Esta vista representa la organización lógica de la infraestructura.

Packet Tracer también ofrece una vista física (*Physical Workspace*), que permite representar la ubicación de equipos, racks, edificios y otros elementos físicos, según las funciones disponibles en la versión utilizada.

### 2.3. Panel de selección de dispositivos

En la parte inferior izquierda se encuentra el panel desde el cual se seleccionan los equipos que se incorporarán al proyecto. La selección se organiza por categorías:

| Categoría | Descripción | Ejemplos |
|---|---|---|
| **Network Devices** | Dispositivos que interconectan equipos y redes. | Routers, switches, hubs, dispositivos inalámbricos y de seguridad. |
| **End Devices** | Equipos finales que originan o reciben información. | PC, laptop, servidor, impresora y teléfono IP. |
| **Components** | Componentes y módulos compatibles con determinados dispositivos. | Módulos de interfaz y otros componentes. |
| **Connections** | Medios y tipos de conexión para enlazar dispositivos. | Cable directo, cruzado, fibra y consola. |

Al seleccionar una categoría, el panel contiguo muestra los modelos disponibles. Para agregar uno al proyecto, se puede seleccionar el dispositivo y luego hacer clic en el área de trabajo, o arrastrarlo hasta la ubicación deseada.

### 2.4. Modos de operación: Realtime y Simulation

Packet Tracer dispone de dos modos principales:

- **Realtime:** la red funciona de manera continua y los eventos se procesan automáticamente. Es el modo habitual para construir la topología y comprobar la conectividad.
- **Simulation:** permite controlar el avance de los eventos y observar el recorrido de los paquetes. Se pueden filtrar protocolos y revisar información sobre el procesamiento de cada evento.

El modo Simulation es especialmente útil para estudiar protocolos como ARP, ICMP, DHCP y DNS, siempre que estén soportados por los dispositivos y servicios simulados.

### 2.5. Controles y panel de eventos

Al trabajar en Simulation aparecen controles para iniciar, detener o avanzar los eventos. También se muestra una lista que permite seleccionar un paquete y consultar información sobre su procesamiento en los distintos dispositivos.

Por ejemplo, al ejecutar un `ping` entre dos computadoras, se pueden observar los eventos relacionados con la resolución de direcciones mediante ARP y el intercambio de mensajes ICMP.

---

## 3. Requerimientos del proyecto

El objetivo de esta práctica es construir y comprobar una **red LAN con tres switches interconectados de manera redundante**.

La red deberá contar con los siguientes dispositivos:

| Segmento | Switch asignado | Dispositivos conectados |
|---|---|---:|
| Servidores | `servers` | 3 servidores |
| Taller | `taller` | 4 computadoras |
| Aulas | `aulas` | 6 computadoras |
| **Total** | **3 switches** | **13 dispositivos finales** |

Requisitos funcionales:

1. Cada switch debe conectar los dispositivos de su segmento.
2. Los hosts, incluidos los servidores, deben poder comunicarse entre sí.
3. La infraestructura debe conservar conectividad entre segmentos ante la interrupción de uno de los enlaces que interconectan los switches.
4. Deben existir caminos físicos alternativos entre los switches.

### 3.1. ¿Qué topología se busca implementar?

Los tres switches se conectarán entre sí formando un triángulo. Desde el punto de vista físico, esta interconexión constituye una **topología en malla completa entre tres switches**: cada switch tiene un enlace directo con los otros dos.

No es una topología de anillo. En un anillo, cada nodo se conecta normalmente con dos nodos vecinos formando un circuito cerrado, sin que todos los pares de nodos tengan necesariamente un enlace directo. En este proyecto, los tres switches están conectados entre sí mediante los tres enlaces posibles.

La malla aporta redundancia: si se interrumpe uno de los enlaces entre switches, todavía puede existir un camino alternativo entre los otros dos segmentos.

> **Importante:** la redundancia de enlaces no protege frente a la falla completa de un switch. Si el switch `taller` deja de funcionar, los cuatro equipos conectados exclusivamente a él perderán su conexión con la red. Para tolerar también la falla de un switch sería necesario un diseño diferente, con equipos o conexiones redundantes para los hosts.

### 3.2. Consideración sobre los bucles de capa 2

Al conectar switches de capa 2 formando un triángulo, se crea un camino físico cerrado. Si todos los enlaces reenvían tramas simultáneamente dentro de la misma VLAN, pueden producirse **bucles de capa 2**, que provocan tormentas de broadcast, duplicación de tramas e inestabilidad en las tablas MAC.

Para evitarlo, los switches gestionables utilizan **Spanning Tree Protocol (STP)** o alguna de sus variantes. STP identifica los caminos redundantes y bloquea lógicamente uno o más puertos para evitar bucles, manteniendo una ruta alternativa disponible para activarla si falla un enlace.

Por lo tanto, aunque físicamente se observen tres enlaces entre los switches, no necesariamente los tres estarán reenviando tráfico de usuario al mismo tiempo. El comportamiento dependerá de la configuración y de la variante de STP implementada por los equipos simulados.

---

## 4. Desarrollo de la práctica

### 4.1. Incorporar y configurar los switches

![Selección de switches](./images/img04.png)

El primer paso consiste en incorporar los tres switches que formarán la infraestructura principal.

1. En el panel de selección de dispositivos, ingresar en **Network Devices**.
2. Seleccionar la categoría **Switches**.
3. Revisar los modelos disponibles y elegir el **Cisco 2960** para esta práctica.
4. Hacer clic sobre el modelo y luego sobre el área de trabajo para colocarlo. También se puede arrastrar el dispositivo.
5. Repetir el procedimiento hasta incorporar tres switches.
6. Ubicarlos dejando espacio suficiente para conectar los hosts y representar los enlaces entre ellos.

El Cisco Catalyst 2960 representa una familia de switches Ethernet de capa 2 orientados principalmente a redes de acceso. Los modelos de esa familia varían en cantidad de puertos, capacidades y características. En Packet Tracer se utiliza un modelo simplificado con las funciones que el simulador tiene implementadas.

#### Identificar los switches

Para facilitar la lectura del diagrama, se asignará a cada switch un nombre visible:

- `servers`
- `taller`
- `aulas`

En Packet Tracer es importante distinguir entre **Display Name** y **hostname**:

- **Display Name:** etiqueta que aparece junto al dispositivo en el área de trabajo. Sirve para identificarlo visualmente.
- **Hostname:** nombre configurado en el sistema operativo del dispositivo. Se utiliza, por ejemplo, en el prompt de la CLI.

Cambiar el Display Name no modifica automáticamente el hostname. Para configurar el hostname, se debe ingresar a la pestaña **CLI** del switch y utilizar los comandos correspondientes, si el modelo simulado los admite.

Ejemplo:

```text
Switch> enable
Switch# configure terminal
Switch(config)# hostname servers
servers(config)# end
servers#
```

Repetir el procedimiento para `taller` y `aulas`.

**Resultado esperado:**

![Switches incorporados y etiquetados](./images/img05.png)

### 4.2. Incorporar los dispositivos finales

Una vez colocados los switches, se agregan los equipos que utilizarán la red.

1. En el panel de selección, ingresar en **End Devices**.
2. Seleccionar el tipo de dispositivo correspondiente.
3. Incorporar tres servidores y diez computadoras (PC).
4. Distribuirlos alrededor del switch al que se conectarán.

La distribución será:

- **Switch `servers`:** 3 servidores.
- **Switch `taller`:** 4 PCs.
- **Switch `aulas`:** 6 PCs.

Es recomendable mantener agrupados los equipos de cada segmento y dejar espacio entre los switches para que los enlaces sean fáciles de identificar.

![Dispositivos finales distribuidos](./images/img06.png)

> En esta etapa solamente se agregan los dispositivos al diagrama. Todavía no se configuró el direccionamiento IP ni se comprobó la conectividad.

### 4.3. Conectar los dispositivos

Para conectar los equipos, ingresar en la categoría **Connections**, ubicada en el panel de selección.

Packet Tracer ofrece diferentes tipos de conexiones. Entre las más utilizadas se encuentran:

![Tipos de conexiones disponibles](./images/img07.png)

| Tipo de conexión | Uso general |
|---|---|
| **Automatically Choose Connection Type** | El simulador selecciona un tipo de conexión apropiado para los puertos elegidos. |
| **Console** | Conexión de administración utilizada para acceder a la CLI mediante el puerto de consola. No transporta el tráfico normal de la LAN. |
| **Copper Straight-Through** | Cable de cobre directo, utilizado tradicionalmente para conectar dispositivos de distinto tipo, por ejemplo, una PC con un switch. |
| **Copper Cross-Over** | Cable de cobre cruzado, utilizado tradicionalmente para conectar dispositivos de tipo similar, por ejemplo, dos switches. |
| **Fiber** | Enlace de fibra óptica entre interfaces compatibles. |
| **Phone / Coaxial / Serial** | Otros medios disponibles para escenarios y dispositivos específicos, según la versión y el modelo. |

En equipos reales, la necesidad de utilizar un cable directo o cruzado depende de las interfaces y de si admiten Auto-MDI/MDIX. Muchos equipos modernos detectan automáticamente el tipo de conexión. Packet Tracer representa estas diferencias según el modelo simulado.

#### Conectar los servidores

1. Seleccionar **Copper Straight-Through**.
2. Hacer clic en el switch `servers`.
3. Elegir un puerto Ethernet disponible, por ejemplo, `FastEthernet0/1`.
4. Hacer clic en el primer servidor y seleccionar su interfaz `FastEthernet0`.
5. Repetir el procedimiento con los otros dos servidores, utilizando puertos diferentes del switch.

#### Conectar las PCs de Taller

1. Mantener seleccionado el cable **Copper Straight-Through**.
2. Conectar cada una de las cuatro PCs al switch `taller`.
3. Utilizar un puerto distinto para cada equipo.

#### Conectar las PCs de Aulas

1. Conectar las seis PCs restantes al switch `aulas`.
2. Utilizar un puerto Ethernet disponible para cada una.

La conexión entre un host y un switch se realiza normalmente mediante un cable directo en este tipo de práctica.

![Conexiones entre hosts y switches](./images/img08.png)

#### Interconectar los switches

Para que los dispositivos de los tres segmentos puedan comunicarse a través de los switches, es necesario conectar los propios switches.

Se establecerán enlaces entre cada par:

- `servers` ↔ `taller`
- `taller` ↔ `aulas`
- `aulas` ↔ `servers`

Para cada enlace:

1. Seleccionar el tipo de cable apropiado para las interfaces disponibles. En esta práctica se puede utilizar **Copper Cross-Over** para representar la conexión tradicional entre switches.
2. Seleccionar un puerto Ethernet disponible en el primer switch.
3. Seleccionar un puerto Ethernet disponible en el segundo switch.
4. Repetir hasta completar los tres enlaces.

Al finalizar, los tres switches deben formar un triángulo.

![Topología física completa](./images/img09.png)

### 4.4. Identificar la topología resultante

La interconexión de los tres switches constituye una **malla completa de tres nodos**. Cada switch dispone de un enlace directo con los otros dos.

La cantidad de enlaces de una malla completa no dirigida se calcula mediante:

\[
L = \frac{n(n-1)}{2}
\]

Donde `L` es la cantidad de enlaces y `n` la cantidad de nodos.

Para este proyecto:

\[
L = \frac{3(3-1)}{2} = 3
\]

Por lo tanto, se necesitan tres enlaces para conectar directamente todos los pares de switches.

Si se interrumpe el enlace entre `servers` y `taller`, por ejemplo, todavía existe el camino alternativo:

`servers` → `aulas` → `taller`

Para que ese camino alternativo pueda utilizarse correctamente en una red de capa 2, se necesita un mecanismo que gestione la redundancia y evite bucles, como STP.

---

## 5. Configuración lógica y comprobación de conectividad

La conexión física no es suficiente para garantizar la comunicación. También es necesario configurar el direccionamiento IP y verificar que los equipos pertenezcan a la misma red lógica o que exista un dispositivo de capa 3 que permita el enrutamiento entre subredes.

Para esta práctica, una alternativa sencilla es utilizar una única subred IPv4 y mantener los puertos de los switches en la misma VLAN. Así, los hosts pueden comunicarse a través de los switches sin necesitar un router, siempre que sus direcciones y máscaras sean compatibles.

### 5.1. Ejemplo de direccionamiento IPv4

Se puede utilizar la red privada `192.168.10.0/24`.

| Dispositivo | Dirección IPv4 | Máscara |
|---|---|---|
| Servidor 1 | `192.168.10.10` | `255.255.255.0` |
| Servidor 2 | `192.168.10.11` | `255.255.255.0` |
| Servidor 3 | `192.168.10.12` | `255.255.255.0` |
| PC Taller 1 | `192.168.10.21` | `255.255.255.0` |
| PC Taller 2 | `192.168.10.22` | `255.255.255.0` |
| PC Taller 3 | `192.168.10.23` | `255.255.255.0` |
| PC Taller 4 | `192.168.10.24` | `255.255.255.0` |
| PC Aulas 1 | `192.168.10.31` | `255.255.255.0` |
| PC Aulas 2 | `192.168.10.32` | `255.255.255.0` |
| PC Aulas 3 | `192.168.10.33` | `255.255.255.0` |
| PC Aulas 4 | `192.168.10.34` | `255.255.255.0` |
| PC Aulas 5 | `192.168.10.35` | `255.255.255.0` |
| PC Aulas 6 | `192.168.10.36` | `255.255.255.0` |

En este ejemplo no se configura una puerta de enlace porque todos los equipos pertenecen a la misma subred y no necesitan salir de ella para realizar las pruebas. Para acceder a otras redes sería necesario incorporar y configurar un router o un switch de capa 3, además de definir la puerta de enlace correspondiente.

Para asignar una dirección IP a cada PC o servidor:

1. Abrir el dispositivo con doble clic.
2. Ingresar en la pestaña **Desktop**.
3. Seleccionar **IP Configuration**.
4. Completar los campos **IPv4 Address** y **Subnet Mask**.
5. Repetir el procedimiento para los trece dispositivos, evitando direcciones duplicadas.

### 5.2. Comprobar la comunicación

Una vez configuradas las direcciones IP:

1. Abrir una PC.
2. Ingresar en **Desktop** y luego en **Command Prompt**.
3. Ejecutar `ping` hacia la dirección IP de otro dispositivo.

Ejemplo:

```text
ping 192.168.10.10
```

La respuesta indica que el equipo de destino respondió a los mensajes ICMP Echo Request. Si no hay respuesta, se debe revisar el direccionamiento, la máscara, el estado de los enlaces, la VLAN y la configuración de los dispositivos.

Realizar pruebas entre:

- Dos dispositivos conectados al mismo switch.
- Una PC de `taller` y otra de `aulas`.
- Una PC de `taller` y un servidor.
- Una PC de `aulas` y un servidor.

La primera comunicación puede generar eventos ARP para resolver la dirección MAC correspondiente a la IP de destino. Por eso, al analizar el primer `ping`, es posible observar primero el intercambio ARP y luego los mensajes ICMP.

### 5.3. Comprobar la redundancia ante la caída de un enlace

Para verificar el requisito de tolerancia a la interrupción de un enlace:

1. Confirmar que los hosts de los tres segmentos se comunican correctamente.
2. Identificar uno de los enlaces que conecta dos switches.
3. Interrumpir ese enlace desde Packet Tracer, por ejemplo, eliminando el cable o apagando administrativamente uno de los puertos utilizados.
4. Esperar a que STP detecte el cambio y recalcule la topología, si corresponde.
5. Volver a ejecutar `ping` entre equipos conectados a switches diferentes.
6. Comprobar si la comunicación se restablece mediante el camino alternativo.
7. Restaurar el enlace y observar el comportamiento de STP.

El tiempo de recuperación depende de la variante y configuración de STP, del estado de los puertos y de las funciones soportadas por el modelo simulado. No debe suponerse que la recuperación será instantánea.

> Esta prueba verifica la tolerancia a la falla de un enlace entre switches. No demuestra tolerancia a la falla de un switch completo ni de los enlaces de acceso que conectan los hosts.

---

## 6. Conclusión

En esta práctica se diseñó una red LAN compuesta por tres switches y trece dispositivos finales, distribuidos en tres segmentos: `servers`, `taller` y `aulas`.

Los switches se interconectaron mediante una topología física de malla completa, con tres enlaces entre los tres equipos. Este diseño ofrece caminos alternativos ante la interrupción de un enlace entre switches.

También se identificó la importancia de configurar el direccionamiento IP y de utilizar mecanismos como **Spanning Tree Protocol (STP)** para evitar bucles de capa 2 cuando existen conexiones redundantes.

La práctica permite relacionar conceptos de topologías, dispositivos de interconexión, medios de transmisión, direccionamiento IP, protocolos y disponibilidad de una red. Packet Tracer permite observar y probar el comportamiento de la infraestructura antes de implementar un diseño equivalente en un entorno físico.

## 7. Conceptos clave

- **Cisco Packet Tracer:** simulador educativo para diseñar, configurar y analizar redes.
- **Logical Workspace:** área donde se construye el diagrama lógico.
- **End Device:** equipo final que origina o recibe información.
- **Switch:** dispositivo que interconecta equipos dentro de una LAN y reenvía tramas utilizando direcciones MAC.
- **Router:** dispositivo que interconecta redes IP y reenvía paquetes según su tabla de enrutamiento.
- **Display Name:** etiqueta visual de un dispositivo en el diagrama.
- **Hostname:** nombre configurado en el sistema operativo del dispositivo.
- **Copper Straight-Through:** cable de cobre directo, utilizado tradicionalmente para conectar dispositivos de distinto tipo.
- **Copper Cross-Over:** cable de cobre cruzado, utilizado tradicionalmente para conectar dispositivos de tipo similar.
- **Malla completa:** topología en la que cada nodo tiene un enlace directo con todos los demás.
- **Redundancia:** existencia de recursos o caminos alternativos para reducir el impacto de una falla.
- **STP:** protocolo que evita bucles de capa 2 y administra enlaces redundantes.
- **Ping:** utilidad que permite comprobar conectividad IP mediante mensajes ICMP Echo Request y Echo Reply.
- **ARP:** protocolo utilizado en IPv4 para resolver una dirección IP de la red local a una dirección MAC.