# Introducción a Bases de Datos

## Descripción

Este directorio contiene la documentación, los conceptos teóricos y las prácticas correspondientes al curso **Introducción a Bases de Datos**, perteneciente al recorrido de formación Linux Engineer.

El objetivo es comprender los fundamentos de las bases de datos, su organización y los mecanismos utilizados para almacenar, consultar, modificar y administrar información de manera estructurada.

Los contenidos se orientan a desarrollar una base técnica que permita trabajar con sistemas de gestión de bases de datos (DBMS), comprender el lenguaje SQL y relacionar estos conocimientos con tareas de administración de sistemas, infraestructura y servicios.

## Objetivos

- Comprender los conceptos fundamentales de bases de datos y sistemas de gestión de bases de datos.
- Diferenciar los modelos de bases de datos y sus principales características.
- Reconocer entidades, atributos, registros, tablas y relaciones.
- Comprender el modelo relacional y las restricciones de integridad.
- Diseñar estructuras de datos a partir de requerimientos.
- Utilizar SQL para definir estructuras y consultar, insertar, modificar y eliminar información.
- Comprender la importancia de las claves primarias, las claves foráneas y la normalización.
- Reconocer conceptos básicos de seguridad, mantenimiento y respaldo de bases de datos.

## Contenidos

### 1. Fundamentos de bases de datos

- Concepto y propósito de una base de datos.
- Diferencias entre datos e información.
- Sistemas de gestión de bases de datos (DBMS).
- Funciones y responsabilidades de un DBMS.
- Ventajas y limitaciones del almacenamiento estructurado.
- Diferencias entre archivos tradicionales y bases de datos.
- Introducción a los modelos de bases de datos.

### 2. Modelo relacional

- Conceptos de relación, tabla, fila y columna.
- Atributos, dominios y tipos de datos.
- Clave primaria (*Primary Key*).
- Clave foránea (*Foreign Key*).
- Claves candidatas y claves compuestas.
- Relaciones uno a uno, uno a muchos y muchos a muchos.
- Integridad de entidad e integridad referencial.
- Restricciones de unicidad, obligatoriedad y validación.

### 3. Diseño y modelado de datos

- Análisis de requerimientos.
- Identificación de entidades y atributos.
- Relaciones y cardinalidades.
- Diagramas entidad-relación (DER).
- Conversión del modelo conceptual al modelo relacional.
- Redundancia e inconsistencias de datos.
- Introducción a la normalización.
- Formas normales: primera, segunda y tercera forma normal.

### 4. Lenguaje SQL

- Introducción al lenguaje SQL.
- Definición de estructuras mediante DDL.
- Manipulación de datos mediante DML.
- Consultas mediante `SELECT`.
- Filtrado y ordenamiento con `WHERE` y `ORDER BY`.
- Operadores lógicos y de comparación.
- Funciones de agregación: `COUNT`, `SUM`, `AVG`, `MIN` y `MAX`.
- Agrupamiento mediante `GROUP BY` y filtrado con `HAVING`.
- Combinación de tablas mediante `JOIN`.
- Subconsultas.
- Inserción, actualización y eliminación de registros.

### 5. Administración y mantenimiento

- Introducción a los sistemas de gestión de bases de datos.
- Creación y eliminación de bases de datos y tablas.
- Gestión de usuarios y permisos, según el DBMS utilizado.
- Restricciones y validación de datos.
- Introducción a las transacciones y sus propiedades ACID.
- Conceptos básicos de índices y su impacto en las consultas.
- Copias de seguridad y restauración.
- Consideraciones iniciales de seguridad y mantenimiento.

## Prácticas y ejercicios

Las prácticas de este directorio se incorporarán a medida que avance el curso. Se priorizarán ejercicios que permitan aplicar los conceptos teóricos y documentar los resultados obtenidos.

Entre las actividades previstas se encuentran:

- Diseño de una base de datos a partir de un escenario concreto.
- Creación de tablas y definición de claves y restricciones.
- Inserción y modificación de registros.
- Elaboración de consultas SQL simples y complejas.
- Relación de tablas mediante claves foráneas y operaciones `JOIN`.
- Análisis de problemas de redundancia e integridad.
- Pruebas de respaldo y restauración, si forman parte del entorno práctico.

Cada práctica incluirá, cuando corresponda, el objetivo, el procedimiento, las consultas utilizadas, los resultados obtenidos y las observaciones relevantes.

## Tecnologías y herramientas

Las herramientas concretas dependerán del entorno definido durante el curso. Entre las posibles tecnologías se encuentran:

- **SQL:** lenguaje para definir, consultar y manipular datos.
- **PostgreSQL:** sistema de gestión de bases de datos relacional.
- **MySQL / MariaDB:** alternativas de gestión de bases de datos relacionales.
- **Terminal Linux:** ejecución de herramientas y conexión con servicios de bases de datos.
- **Git y GitHub:** versionado y documentación del material.

La inclusión de una tecnología en esta lista no implica que todas hayan sido utilizadas durante el curso; se documentarán las herramientas efectivamente empleadas en cada práctica.

## Propósito dentro del portfolio

Este material forma parte de un portfolio técnico orientado a documentar el aprendizaje práctico en Linux, infraestructura y tecnologías relacionadas.

Comprender las bases de datos complementa los conocimientos de administración de sistemas, ya que muchos servicios y aplicaciones dependen de ellas para almacenar información, gestionar usuarios, procesar solicitudes y mantener la persistencia de los datos.

El objetivo es construir documentación clara, verificable y progresiva que refleje tanto los fundamentos teóricos como la aplicación práctica de los conocimientos adquiridos.