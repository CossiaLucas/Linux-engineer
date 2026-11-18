# Modelo de capas OSI

El modelo `OSI` describe las etapas en un proceso de comunicación, pero no determina el cómo. Un modelo de `referencia` nos ayuda a comprender los procesos y que se realiza en cada capa y nos proporciona los siguientes beneficios:

* Facilita la comprensión al dividir un problema complejo en partes más simples.
* Evita los problemas de compatibilidad, por ejemplo de red.
* Detalla las capas para su mejor aprendizaje
* Proporciona a los fabricantes un conjunto de estándares, que aseguran una mayor compatibilidad entre los distintos tipos de tecnología.

Ahora vamos a explicar y detallar cada una de las 7 capas del modelo.

--- 

# Capa 7 - Aplicacion

En esta capa se ubican las aplicaciones que hacen uso de los servicios de red. Es donde el usuario u otros servicios generan los `mensajes` que pasarán a las capas inferiores.

Las aplicaciones de esta capa no solo generan los mensajes, sino que también los **interpretan**.

Un ejemplo puede ser el cliente de email: 
Un usuario puede escribir y enviar un correo electrónico usando el cliente Thunderbird. El destinatario puede recibir y leer el correo electrónico usando Outlook.

> Aclaracion: Cuando hablamos de mensajes no nos referimos estrictamente a mensajes escritos, sino a la información que genera la aplicación y que debe ser enviada a otro host.

# Capa 6 - Presentacion

Su objetivo es encargarse de la representación de la información, de manera que, aunque distintos equipos puedan tener **diferentes representaciones internas** de caracteres, números, sonido o imágenes, los datos lleguen de manera `reconocible`.

En esta capa es donde se comienzan a aplicar los protocolos de aplicación. Algunos protocolos pueden ser:

## Protocolo para transferencia simple de correo (`SMTP`)

Es el protocolo usado para el envío de correos electrónicos.

Aunque el mensaje que envie el usuario sea simple, ej.

``` text
Destinatario: manuel@gmail.com
Cuerpo del mensaje: Hola!
```

La aplicación enviará un mensaje mucho más `complejo`, y normalmente, `invisible` al usuario.

``` text
S: 220 Servidor SMTP
C: HELO miequipo.midominio.com
S: 250 Hello, please to meet you
C: MAIL FROM: <yo@midominio.com>
S: 250 Ok
C: RCPT TO: <manuel@gmail.com>
S: 250 Ok
C: DATA
S: 354 End data with <CR><LF>.<CR><LF>
C: Subject: Campo de asunto
C: From: yo@midominio.com
C: To: manuel@gmail.com
C:
C: Hola!
C: .
C: <CR><LF>.<CR><LF>
S: 250 Ok: queued as 12345
C: quit
S: 221 Bye
```

## El Protocolo de transferencia de hipertexto (`HTTP`)

El mensaje que se envía es la url que se ingresa en la barra de direcciones de un navegador.

``` text
url: https://youtube.com/
```

El mensaje que recibirá el servidor es la solicitud `GET` del contenido del directorio raíz alojado en un servidor web bajo el nombre de host ‘youtube.com’

```  text
GET / HTTP/1.1
Host: youtube.com
Accept-Language: es
```

## Capa 5 - Sesion

Establece, administra y finaliza las `conexiones` entre las aplicaciones locales y las remotas.

Esta capa también permite:

* **Cifrar** los datos y comprimirlos.
* **Establece y finaliza** las conexiones.
* **Proporciona** sus servicios a la capa de presentación.
* **Sincroniza el diálogo** entre las capas de presentación de los dos hosts y administra su intercambio de datos.

## Capa 4 - Transporte 

La capa de transporte es la responsable de las comunicaciones de extremo a extremo entre dos dispositivos. El mensaje que viene de la `capa de sesión` no se envía como tal, sino que se fragmenta en lo que se denomina `segmentos`, piezas más pequeñas que juntas componen el mensaje.

Los `segmentos` se reciben en el destino y el proceso de armado del mensaje a partir de los segmentos se produce en la misma capa en el extremo opuesto.

La capa de transporte es también la responsable del **control de flujo**y del **control de errores**.

## Capa 3 - Red

Esta capa es responsable del **direccionamiento lógico** y el dominio del enrutamiento. Su misión es conseguir que los datos lleguen desde el origen al destino aunque no tengan conexión directa.

En esta capa los segmentos que llegan de la capa de transporte se encapsulan en lo que se denomina `paquete de datos`, y llevan consigo dos datos fundamentales: **la dirección del host emisor y la dirección del host receptor**.

Esta información permitirá a los protocolos y dispositivos de enrutamiento hacer llegar los paquetes entre hosts que estén en redes distintas.

## Capa 2 - Enlace de datos

Proporciona direccionamiento físico y procedimientos de acceso a medios.

La capa de enlace de datos es muy similar a la `capa de red`, salvo que lo que hace es facilitar la transferencia de datos entre dos dispositivos ubicados en una **misma** red física.

La capa de enlace de datos toma los paquetes de la capa de red y genera trozos más pequeños denominados `tramas`. Estas tramas, así como los paquetes, tienen direcciones de **origen** y **destino**, pero en lugar de ser una dirección lógica es una dirección física, que es como se identifican las interfaces de red dentro de un segmento físico de red que se puede representar a partir de la topología.

El direccionamiento lógico permite a un paquete llegar entre segmentos de red físicos y lógicos **distintos**. Una trama solo llega a interfaces de red a las que se pueda acceder físicamente, que estén conectados entre sí, por ejemplo, los hosts que orbitan un switch.

## Capa 1 - Fisica

La capa física define las especificaciones eléctricas, mecánicas, de procedimiento y funcionales para activar, mantener y desactivar el enlace físico entre sistemas finales.

Básicamente son los medios de transmisión de datos, por ejemplo:

* Cables de cobre.
* Cables de fibra óptica.
* Ondas de radio (wifi, bluetooth, etc.)