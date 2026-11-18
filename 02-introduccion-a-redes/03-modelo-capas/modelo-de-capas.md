# Modelo de capas

Los modelos en capas se utilizan para **ayudar a visualizar** la interacción entre diversos protocolos.

Un modelo en capas describe el **funcionamiento de los protocolos** que se produce en `cada capa` y la interacción de los protocolos con las capas que se encuentran por encima y por debajo de ellas, describiendo, además, el flujo de información en el proceso de comunicación en uno u otro sentido.

* Ayuda en el diseño de protocolos, ya que los protocolos que operan en una capa específica tienen información definida según la cual actúan, y una interfaz definida para las capas superiores e inferiores.
* Fomenta la competencia, ya que los productos de distintos proveedores pueden trabajar en conjunto.
* Evita que los cambios en la tecnología o en las capacidades de una capa afecten otras capas superiores e inferiores.
* Proporciona un lenguaje común para describir las funciones y capacidades de redes.

## Modelo de protocolos

Este modelo describe **la estructura** de un conjunto de protocolos determinado.
El conjunto `jerárquico` de protocolos relacionados representa toda la funcionalidad requerida para todos los elementos, tanto software como hardware, de una red de datos.

El modelo `TCP/IP` es un modelo de protocolo, porque describe las funciones que tienen lugar en cada capa de protocolos dentro de una suite `TCP/IP`.

## Modelo de referencia

Este modelo es coherente con todos los tipos de servicios y protocolos de red al describir qué es lo que **se debe hacer** en una capa determinada, pero no determina la forma en que se debe lograr.

El objetivo principal de un modelo de referencia es ayudar a lograr un mejor entendimiento de las funciones y procesos involucrados.

El modelo `OSI` es el modelo de referencia de internetwork más conocido. Se usa para diseño de redes de datos, especificaciones de funcionamiento y resolución de problemas.