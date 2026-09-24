# Instalación de Linux

## Objetivo

Instalar y realizar la configuración inicial de una distribución Linux
en un entorno virtualizado.

## Entorno

| Recurso       | Configuración           |
| ------------- | ----------------------- |
| Virtualizador | VirtualBox              |
| Distribución  | Debian                  |
| CPU           | 2 vCPU                  |
| RAM           | 2 GB                    |
| Disco         | 20 GB                   |
| Red           | NAT                     |
| Arquitectura  | amdx6                   |


## Preparacion de la ISO

Para realizar la práctica se utilizó Debian 13.7.0 con arquitectura amdx64. La instalación se realizó sobre una máquina virtual creada mediante VirtualBox. Seleccione esta ISO pues es una de las distribuciones mas importantes de Linux ademas su instalacion es bastante sencilla pero podemos tocar conceptos importantes como las particiones de dico o usuario root

## Procedimiento

### **1. Creación de la máquina virtual**

Una vez instalado y configurado **VirtualBox**, y descargada la imagen ISO de **Debian**, podemos comenzar con la creación de la máquina virtual.

El objetivo de esta práctica es crear un entorno virtual donde podamos instalar Debian y realizar posteriormente distintas tareas de administración de sistemas sin modificar el sistema operativo principal de nuestra computadora.

Primero abriremos VirtualBox y seleccionaremos el botón **"Nueva"**.

![Creación de la máquina virtual](./screenshots/img01.png)

En la primera pantalla de configuración debemos definir algunos parámetros básicos de la máquina virtual:

![Primera configuración](./screenshots/img02.png)

* **Nombre:** utilizaremos `Debian13` para identificar fácilmente la máquina.
* **Imagen ISO:** seleccionaremos la imagen ISO de Debian que descargamos previamente.
* **Instalación desatendida:** desmarcaremos la opción **"Proceed with unattended installation"**.

La instalación desatendida permite que VirtualBox automatice gran parte del proceso de instalación del sistema operativo utilizando previamente definidos, como el usuario y la contraseña. En esta práctica no utilizaremos esta modalidad porque queremos realizar manualmente la instalación de Debian y conocer cada una de sus etapas.

---

A continuación accederemos a la sección **"Specify virtual hardware"**, donde definiremos los recursos de hardware que tendrá disponible nuestra máquina virtual.

![Configuración del hardware virtual](./screenshots/img03.png)

Para esta práctica configuraremos:

* **Procesadores (CPU):** 2 núcleos.
* **Memoria RAM:** se asignará una cantidad adecuada para ejecutar Debian y las herramientas que utilizaremos durante las prácticas, con 2 a 4 GB es suficiente.
* **Disco virtual:** se configurará posteriormente en la sección correspondiente.

Los procesadores virtuales representan los núcleos de CPU que VirtualBox permitirá utilizar a la máquina virtual. Asignar dos núcleos proporciona recursos suficientes para una instalación de Debian orientada a prácticas de administración y, al mismo tiempo, evita asignar una cantidad excesiva de recursos al entorno virtual.

La memoria RAM cumple una función similar: es utilizada temporalmente por el sistema operativo y las aplicaciones que se encuentran en ejecución. La cantidad asignada debe ser suficiente para que Debian funcione correctamente, pero debemos evitar asignar una cantidad que deje al sistema operativo anfitrión sin recursos suficientes.

Para esta práctica utilizaremos **2 GB de RAM**, una cantidad adecuada para un entorno de aprendizaje básico.

---

La siguiente sección corresponde a **"Set up unattended guest OS installation"**.

En nuestro caso esta opción no será utilizada, ya que anteriormente deshabilitamos la instalación desatendida.

La instalación desatendida permite automatizar el proceso de instalación del sistema operativo. VirtualBox puede proporcionar al instalador determinados datos, como:

* nombre de usuario;
* contraseña;
* nombre del equipo;
* configuración básica del sistema.

Esto resulta útil cuando necesitamos crear muchas máquinas virtuales con una configuración similar.

Sin embargo, para esta práctica realizaremos la instalación manualmente. De esta manera podremos observar y configurar directamente los diferentes pasos del instalador de Debian.

---

Finalmente llegaremos a la sección **"Specify virtual hard disk"**, donde configuraremos el almacenamiento disponible para la máquina virtual.

Para esta práctica utilizaremos un **disco virtual de 25 GB**.

El disco virtual es un archivo almacenado en el sistema operativo anfitrión que VirtualBox presenta a Debian como si fuera un disco físico.

Durante la instalación, Debian podrá particionar este disco y crear sobre él las estructuras necesarias para almacenar:

* el sistema operativo;
* programas instalados;
* archivos de configuración;
* usuarios y sus archivos personales;
* logs y otros datos generados durante el funcionamiento del sistema.

El disco virtual no corresponde a un disco físico independiente. VirtualBox se encarga de administrar el archivo que representa dicho disco y de presentarlo a la máquina virtual como un dispositivo de almacenamiento.

Una vez confirmada la configuración, finalizamos la creación de la máquina virtual.

### 2. Instalación del sistema.




3. Configuración de red.
4. Actualización de paquetes.
5. Creación del usuario.
6. Configuración inicial.

## Resultado

Sistema instalado y funcionando correctamente.


