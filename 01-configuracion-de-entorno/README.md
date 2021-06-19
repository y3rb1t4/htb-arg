# Configuración de mi entorno de linux

![GitHub](./images/logo.jpg)

---

<br/><br/>

## Tabla de Contenidos:

1. [Introduccion (1)](#1-introduccion)
2. [Configurando La Maquina Virtual ](#2-configurando-la-maquina-virtual)
3. [Configurando de Docker ](#2-configurando-la-maquina-virtual)
4. [Primeros Pasos en mi Kali Linux ](#3-primeros-pasos-en-mi-kali-linux)
5. [Top 10 De las Primeras Instalaciones](#4-top-10-de-las-primeras-instalaciones)
6. [Python ](#5-python)
7. [Goland](#6-goland)
8. [bashrc & zshrc](#7-bashrc-zshrc)
9. [Tmux](#8-tmux)

<br/><br/>

## `1. Introduccion`

<p>El armado de este capitulo es para preparar el nuestro entorno de trabajo , preparar el sistema operativo necessario de forma adecuada y eficiente.</p>

<p>Cada entorno es diferente, y nunca sabemos con qué nos vamos a encontrar una vez que empezamos a enumerar la red y a descubrir los problemas. Tenemos que compilar/instalar herramientas o descargar scripts específicos en nuestra máquina virtual de ataque durante casi todas las evaluaciones que realizamos. Tener nuestras herramientas configuradas de la mejor manera posible garantizará que no perdamos tiempo en los primeros como estudiantes de pentesting y miembros de hack the box, sino que sólo tengamos que hacer cambios en nuestras máquinas virtuales de evaluación para escenarios específicos que encontremos durante la evaluación.</p>

## Configuracion y eficiencia

<p>Con el tiempo, todos reunimos diferentes experiencias y colecciones de herramientas con las que estamos más familiarizados. Estar estructurado es de suma importancia, ya que aumenta nuestra eficiencia en las pruebas de penetración. La búsqueda de recursos individuales e incluso la necesidad de herramientas adicionales para hacer que estos recursos funcionen en el momento en que se inicie un compromiso puede eliminarse al tener acceso a un entorno precocido, organizado y estructurado. Hacerlo requiere cierta preparación y conocimiento de los diferentes sistemas operativos.</p>

## `2. Configurando La Maquina Virtual`

## ![✔] 1.1 Virtualizacion.

La virtualización es una abstracción de los recursos informáticos físicos. Se pueden abstraer tanto los componentes de hardware como los de software. Un componente informático creado como parte de la virtualización se denomina componente virtual o lógico y puede utilizarse precisamente como su homólogo físico. La principal ventaja de la virtualización es la capa de abstracción entre el recurso físico y la imagen virtual. Esta es la base de varios servicios en la nube, que cada vez son más importantes en el día a día de las empresas. La virtualización debe distinguirse de los conceptos de simulación y emulación.

La virtualización implica la abstracción de los recursos informáticos físicos, como el hardware, el software, el almacenamiento y los componentes de red. El objetivo es hacer que estos recursos estén disponibles a nivel virtual y distribuirlos a los diferentes clientes de una manera tan flexible como orientada a la demanda. Esto debería garantizar una mejor utilización de los recursos informáticos. El objetivo es ejecutar aplicaciones en un sistema que no es compatible con él. En la virtualización se distingue entre:

<ul>
    <li>Hardware virtualization</li>
    <li> Application virtualization </li>
    <li>Storage virtualization </li>
    <li>Data virtualization </li>
    <li>Network virtualization </li>
</ul>

## ![✔] 1.2 Introducción a VMware

### VMware Installation on Linux.

```
htbarg@Kali:~$ sudo apt install build-essential -y
htbarg@Kali:~$ sudo bash ~/Downloads/VMware*.bundle
```

### Introducción a Virtual Box

```
htbarg@Kali:~$ sudo apt install virtualbox virtualbox-ext-pack -y
```

## `3. Configurando de Docker`

## ![✔] 1

## `4. Primeros Pasos en mi Kali Linux`

## `5. Top 10 De Las Primeras Instalaciones en Kali `

## TOP de configuracion para la instalación

1- Python - mostrar como se configura correctamente pip, pip3 y pipx en cuanto a todo lo que se refiere con el manejador de paquetes de python y los principales comandos de uso. La idea es explicar como configurar esto correctamente por primera vez para después no tener quilombos con la instalación de otras tools.

2- Goland

3- Como instalar y configurar correctamente las variables para después también poder instalar correctamente tools hechas en este lenguaje.

4- Configuración y personalización del zshrc (mismo concepto del bashrc) y por ende después que la persona pueda usar uno u otro de acuerdo a su conveniencia

5- Profundizar los visto en el punto 3 y 4 pero profundizando un poco más en como personalizar un poco más la shell.Sin llegar a instalar scripts del tipo ohh my zsh

6- Por último y para no hacer un video de 3hs le dedicaría tiempo a la instalacion y personalización y correcto uso de tmux que claramente es un must.

https://linuxconfig.org/how-to-install-pip-on-kali-linux

https://pipxproject.github.io/pipx/installation/

## `6. Python `

## ![✔] 6.1 Como Instalar pip in Kali Linux

<p>Pip es el gestor de paquetes para el lenguaje de codificación Python. Puede instalarse en un sistema Linux y luego utilizarse en la línea de comandos para descargar e instalar paquetes de Python y sus dependencias necesarias.</p>

<p>Python es un lenguaje común para usar en scripts de hacking, y en Kali Linux, el mayor uso de pip sería para instalar las dependencias necesarias para los programas de hacking de Python. Ya sea que esté desarrollando su propio script o tratando de ejecutar un programa Python de terceros, tener pip en su sistema le permitirá instalar paquetes de dependencia muy fácilmente.</p>

<p>En esta guía, le mostraremos cómo instalar pip para Python 3 en Kali. También te mostraremos los comandos básicos de uso de pip, como la instalación y eliminación de paquetes de software. pip funciona de forma muy parecida al gestor de paquetes de Kali, con el que probablemente ya estés familiarizado.</p>

<p>Vamos a meterle</p>
<ul>
    <li>¿ Cómo instalar pip en Kali ?</li>
    <li>Comandos básicos de uso de pip</li>
</ul>

### Colocar Imagen

| Categoria    |                                                                                                    Requisitos, convenciones o versión de software utilizada                                                                                                    |
| :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| Sistema      |                                                                                                                           Kali Linux                                                                                                                           |
| Software     |                                                                                                                              pip3                                                                                                                              |
| Otro         |                                                                                          Acceso privilegiado a su sistema Linux como root o mediante el comando sudo.                                                                                          |
| Convenciones | # - requiere que los comandos de linux dados sean ejecutados con privilegios de root, ya sea directamente como usuario root o mediante el uso del comando sudo $ - requiere que los comandos de linux dados se ejecuten como un usuario normal sin privilegios |

## ¿ Cómo instalar pip en Kali ?

Abra un terminal y escriba los siguientes comandos para instalar pip para Python 3.

```
$ sudo apt update
$ sudo apt install python3-pip
```

## Comandos básicos de uso de pip

Puede utilizar el comando pip3 desde la terminal para utilizar el gestor de paquetes pip. A continuación hay una lista de comandos pip3 para empezar.

Para ver la versión de pip y verificar que está instalada en el sistema:

```
$ pip3 -V
```

Para instalar un paquete:

```
$ pip3 install package-name
```

Para eliminar un paquete:

```
$ pip3 uninstall package-name
```

Para buscar un paquete concreto:

```
$ pip3 search package-name
```

Para ver qué paquetes están instalados en su sistema:

```
$ pip3 list
```

Para ver información sobre un determinado paquete instalado:

```
$ pip3 show package-name
```

Para acceder al menú de ayuda y ver una lista completa de los comandos pip disponibles:

```
$ pip3 help
```

Estos son probablemente todos los comandos que necesitarás, pero puedes consultar el menú de ayuda para ver algunos más, o para obtener un rápido repaso en caso de que olvides alguno de los comandos.

### Colocar Imagen

## `7. Go Land `

## `8. Bashrc & Zshrc `

## `9. Configuracion de Tmux `


## Python 