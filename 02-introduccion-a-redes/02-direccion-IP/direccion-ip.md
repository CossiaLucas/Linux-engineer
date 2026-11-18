# Direccion IP

Una dirección IP (*Internet Protocol*) es una dirección lógica y jerárquica asignada a una **interfaz de red** para identificarla y permitir su comunicación mediante el protocolo IP. Aunque suele decirse que identifica a un dispositivo, técnicamente identifica una interfaz. Por eso, un equipo puede tener varias direcciones IP si dispone de varias interfaces de red.

En una red podemos distinguir:

- **Topología física:** disposición de los dispositivos y sus conexiones físicas o inalámbricas.
- **Topología lógica:** organización de los dispositivos desde el punto de vista de la comunicación, por ejemplo, mediante direcciones IP y subredes.

La disposición física no determina necesariamente la organización lógica. Varios equipos conectados al mismo switch pueden pertenecer a redes IP diferentes.

### 1.1. Estructura de IPv4

Una dirección IPv4 tiene **32 bits**, agrupados en cuatro bloques de 8 bits llamados *octetos*. Se representa normalmente en decimal, separados por puntos.

```text
192.168.1.10
```

Cada octeto puede tomar valores entre `0` y `255`, ya que 8 bits permiten representar 256 valores posibles.

También puede escribirse con notación CIDR, que indica la longitud del prefijo de red:

```text
192.168.1.10/24
```

`/24` significa que los primeros 24 bits identifican la red y los 8 bits restantes identifican la interfaz dentro de esa red.

## 2. Máscara de red y prefijo

La dirección IPv4 y su máscara permiten determinar a qué red pertenece una interfaz. La máscara separa la parte de red de la parte destinada a identificar hosts:

- Los bits en **1** representan la parte de red.
- Los bits en **0** representan la parte de host.

Ejemplo:

```text
Máscara decimal: 255.255.255.0
Máscara binaria: 11111111.11111111.11111111.00000000
Prefijo CIDR:    /24
```

Con `/24`, los primeros tres octetos corresponden a la red y el último queda para identificar hosts.

> La máscara no representa simplemente una cantidad de direcciones disponibles: define qué bits de la dirección pertenecen al prefijo de red y cuáles quedan para identificar interfaces dentro de esa subred.

### 2.1. Máscaras frecuentes

| Prefijo | Máscara decimal | Bits para hosts | Hosts utilizables habituales |
|---|---|---:|---:|
| `/8` | `255.0.0.0` | 24 | 16.777.214 |
| `/16` | `255.255.0.0` | 16 | 65.534 |
| `/24` | `255.255.255.0` | 8 | 254 |
| `/25` | `255.255.255.128` | 7 | 126 |
| `/26` | `255.255.255.192` | 6 | 62 |
| `/27` | `255.255.255.224` | 5 | 30 |
| `/28` | `255.255.255.240` | 4 | 14 |
| `/29` | `255.255.255.248` | 3 | 6 |
| `/30` | `255.255.255.252` | 2 | 2 |

## 3. ID de red e ID de host

La combinación de una dirección IP y su máscara permite obtener dos datos:

- **ID de red:** identifica la subred a la que pertenece la interfaz.
- **ID de host:** identifica la interfaz dentro de esa subred.

Ejemplo:

```text
Dirección IP:  192.168.1.5
Máscara:       255.255.255.0
Prefijo:       /24
Red:           192.168.1.0/24
ID de host:    5
```

El ID de host no es un identificador global: solo tiene sentido dentro de la subred correspondiente.

### 3.1. Una red física y dos redes lógicas

Supongamos cuatro equipos conectados físicamente al mismo switch:

| Equipo | Dirección IP | Máscara | Red lógica |
|---|---|---|---|
| PC 1 | `192.168.1.5` | `255.255.255.0` | `192.168.1.0/24` |
| PC 2 | `192.168.1.102` | `255.255.255.0` | `192.168.1.0/24` |
| PC 3 | `192.168.3.18` | `255.255.255.0` | `192.168.3.0/24` |
| PC 4 | `192.168.3.22` | `255.255.255.0` | `192.168.3.0/24` |

Los cuatro equipos comparten la conexión física, pero pertenecen a dos subredes IP diferentes. Para comunicarse entre subredes normalmente se necesita un router o un switch multicapa que realice enrutamiento.

## 4. Dirección de red, hosts y broadcast

En una subred IPv4 tradicional se distinguen:

1. **Dirección de red:** identifica a la subred y no se asigna como dirección de host.
2. **Direcciones de host:** se asignan a las interfaces de esa subred.
3. **Dirección de broadcast:** permite enviar un paquete a todos los hosts de la subred. No se asigna como dirección individual de host.

Ejemplo para `192.168.1.5/24`:

| Dato | Valor |
|---|---|
| Dirección IP del ejemplo | `192.168.1.5` |
| Máscara | `255.255.255.0` |
| Prefijo | `/24` |
| ID de red | `192.168.1.0` |
| Primer host utilizable | `192.168.1.1` |
| Último host utilizable | `192.168.1.254` |
| Broadcast | `192.168.1.255` |
| Hosts utilizables | `254` |

Como el prefijo es `/24`, los primeros tres octetos identifican la red. El último octeto identifica las interfaces, excepto los valores reservados para la dirección de red (`.0`) y el broadcast (`.255`).

### 4.1. Otros ejemplos

| Red / prefijo | Máscara | Primer host | Último host | Broadcast |
|---|---|---|---|---|
| `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | `192.168.10.254` | `192.168.10.255` |
| `10.20.0.0/16` | `255.255.0.0` | `10.20.0.1` | `10.20.255.254` | `10.20.255.255` |
| `190.1.0.0/16` | `255.255.0.0` | `190.1.0.1` | `190.1.255.254` | `190.1.255.255` |

## 5. Cómo calcular una subred

1. Identificar la dirección IP y el prefijo o máscara.
2. Determinar cuántos bits corresponden a red y cuántos a hosts.
3. Obtener la dirección de red poniendo en `0` todos los bits de host.
4. Obtener el broadcast poniendo en `1` todos los bits de host.
5. En una subred tradicional, el rango utilizable va desde la dirección posterior a la de red hasta la anterior al broadcast.

Ejemplo:

```text
IP:       10.20.30.45/24
Máscara:  255.255.255.0

Red:              10.20.30.0
Primer host:      10.20.30.1
Último host:      10.20.30.254
Broadcast:        10.20.30.255
Hosts utilizables: 254
```

## 6. Broadcast y dominio de broadcast

El **broadcast IPv4** es un mecanismo de difusión. Un equipo envía un paquete a la dirección de broadcast de su subred para que lo reciban los hosts del dominio correspondiente.

Por ejemplo, en `192.168.1.0/24`, la dirección de broadcast es `192.168.1.255`.

El **dominio de broadcast** es el conjunto de dispositivos que reciben una difusión de capa 2. En redes Ethernet conmutadas, las VLAN suelen delimitar estos dominios: los broadcasts de una VLAN no se propagan automáticamente a otra.

Es habitual que una subred IP coincida con una VLAN y un dominio de broadcast, pero no son conceptos idénticos: la subred pertenece a la organización de capa 3, mientras que la VLAN delimita un dominio de capa 2.

### 6.1. Dirección de red vs. broadcast

- La **dirección de red** identifica a la subred y no se asigna a un host.
- La **dirección de broadcast** se utiliza para difundir tráfico a los dispositivos del dominio correspondiente y tampoco se asigna como dirección individual de host.

## 7. Configuración IP de una interfaz

Para configurar una interfaz IPv4 se especifican, como mínimo:

- **Dirección IP:** identifica la interfaz dentro de la subred.
- **Máscara o prefijo:** determina la subred y el rango de direcciones.

También suelen configurarse:

- **Puerta de enlace predeterminada (gateway):** router o equipo de capa 3 al que se envía el tráfico destinado a otras redes.
- **Servidores DNS:** resuelven nombres de dominio, como `example.com`, a direcciones IP.

Ejemplo:

```text
IP:       192.168.1.20
Máscara:  255.255.255.0
Gateway:  192.168.1.1
DNS:      1.1.1.1
```

La puerta de enlace debe ser alcanzable desde la subred local. Para comunicarse con equipos de la misma subred no se necesita gateway; sí se necesita para alcanzar otras redes. Las direcciones pueden configurarse manualmente (IP estática) o asignarse automáticamente mediante DHCP.

## 8. ¿Cómo decide un equipo a dónde enviar el tráfico?

Un host utiliza su dirección IP y su máscara para determinar si el destino pertenece a su misma subred:

- **Destino en la misma subred:** intenta comunicarse directamente por la red local. En Ethernet utiliza ARP para averiguar la dirección MAC asociada a la IPv4 de destino, si todavía no la conoce.
- **Destino en otra subred:** envía el tráfico a la puerta de enlace predeterminada, que lo enruta hacia otra red.

Por lo tanto, no es correcto afirmar que dos equipos solo pueden comunicarse si pertenecen a la misma subred. También pueden comunicarse entre subredes diferentes si existe enrutamiento y las rutas y reglas de seguridad lo permiten.

## 9. Conceptos clave

| Concepto | Significado |
|---|---|
| Dirección IP | Dirección lógica asignada a una interfaz de red |
| Máscara / prefijo | Separa los bits de red de los bits de host |
| ID de red | Identifica la subred |
| ID de host | Identifica una interfaz dentro de la subred |
| Broadcast | Dirección de difusión IPv4 de la subred |
| Dominio de broadcast | Dispositivos que reciben broadcasts de capa 2 |
| Gateway predeterminado | Próximo salto para llegar a otras redes |
| DNS | Servicio que resuelve nombres de dominio a direcciones IP |

## 10. Aclaraciones importantes

- Una IP identifica una **interfaz de red**, no necesariamente al dispositivo completo.
- Dos equipos en la misma red física pueden pertenecer a subredes IP diferentes.
- Dos equipos en subredes distintas pueden comunicarse si existe un router o equipo de capa 3 que enrute el tráfico.
- No alcanza con comparar visualmente los primeros octetos: hay que conocer la máscara o el prefijo.
- La fórmula `2^h - 2` es la regla habitual para subredes IPv4 tradicionales, con excepciones como `/31` y `/32`.
- `127.0.0.0/8` es el bloque de loopback reservado para comunicaciones internas del propio equipo. No debe tratarse como un rango común para asignar direcciones a hosts de una LAN.