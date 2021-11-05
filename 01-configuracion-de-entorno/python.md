## Python 

### ¿ que es python ?

Es un lenguaje de escritura rápido, escalable, robusta y de código abierto, ventajas que hacen de **Python** un aliado perfecto **para** el Pentesting. Permite plasmar ideas complejas con unas pocas líneas de código, lo que no es posible con otros lenguajes.

Python el 1 de enero del 2020 finalizo oficialmente el soporte a la version 2.7, discontinuaron el software. 

```bash
python                         
Python 2.7.18 (default, Apr 20 2020, 20:30:41) 
[GCC 9.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

En el sistema kali linux viene por defecto instalado python2 y python3 , pero en este caso python ya no da soporte en python2 , pero kali lo dejo a python2 por el tema que hay muchas herramientas hechas en python.

![[Pasted image 20210614222528.png]]

Es unos de los lenguajes mas populares en la programacion y que mejor que lo dice google, en este post no vamos a programar en python. Vamos a ver como preparar nuestro entorno para trabajar con Python.

con el comando which podemos saber donde tenemos la instalacion de python2

```bash
which python2                                                                                                                                             
/bin/python2
```


```bash
ls -l /usr/bin/python2  
lrwxrwxrwx root root 9 B Tue Aug  4 04:22:34 2020  /usr/bin/python2 ⇒ python2.7
```

```bash
ls -l /usr/bin/python2.7
.rwxr-xr-x root root 3.5 MB Mon Apr 20 16:30:41 2020  /usr/bin/python2.7
```

Vemos la version de python3 actualizada al dia de la fecha 

```bash
python3
Python 3.9.2 (default, Feb 28 2021, 17:03:44) 
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

```


## Aprendiendo pip 

### ¿ que es pip ?

**IP** es un acrónimo **que significa** "Paquetes de instalación **PIP**" o "Programa de instalación preferida". Es una utilidad de línea de **comandos** que le permite instalar, reinstalar o desinstalar paquetes PyPI con un **comando** simple y directo: "**pip**".

### Instalando python3-pip

```bash
sudo apt install python3-pip
```

Les dejo un link donde pueden buscar las librerias de pip 

https://pypi.org/project/pip/

## El entorno de zsh

**ZSH**, también llamado **Z shell**, es una versión extendida de **Bourne Shell** (**sh**), con muchas características nuevas y soporte para plugins y temas. Dado que se basa en el mismo shell que **Bash**, **ZSH** tiene muchas de las mismas características. Empezar a utilizarlo es muy sencillo.

### Variables de entorno

Al abrir una ventana de terminal, se inicializa un nuevo proceso zsh, que tiene sus propias variables de entorno. Estas variables son una forma de almacenamiento global para varias configuraciones heredadas por cualquier aplicación que se ejecute durante esa sesión de terminal. Una de las variables de entorno más referenciadas es PATH, que es una lista separada por dos puntos de las rutas de los directorios que Bash buscará siempre que se ejecute un comando sin una ruta completa.

Podemos ver el contenido de una determinada variable de entorno con el comando echo seguido del carácter "$" y un nombre de variable de entorno. Por ejemplo, veamos el contenido de la variable de entorno PATH:

```bash
kali@kali:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Ahora necesitamos trabajar con el archivo .zshrc que se encontraria en la carpeta del usuario y tambien en la carperta del usuario root 

para agregar una ruta al PATH 

```bash
cat .profile

```

![[Pasted image 20210614231636.png]]

```bash
nano .zshrc

```


```bash
nano .zshrc

```

```bash
pip freeze

```

```bash
git clone https://github.com/SecuraBV/CVE-2020-1472.git
```


```bash
git clone https://github.com/loseys/BlackMamba.git
```



## Pyenv 

Instalando Pyenv

En este repositorio de github , nos vamos a dirijir a la seccion deinstalacion de pyenv
https://github.com/pyenv/pyenv

![[Pasted image 20210614234912.png]]

Pero previamente me dice que instale las dependencias, por lo tanto pasamos a instalar dependencias.

https://github.com/pyenv/pyenv/wiki#suggested-build-environment

en este caso para debian

```bash
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

ahora paso a clonar el repo  y lo bajo en el home directory

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

```


Configura el entorno de tu shell para Pyenv

Previamente procedemos agregar las variables a la .zshrc

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc

echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc

echo 'eval "$(pyenv init --path)"' >> ~/.zshrc

```

una vez agregados estas lineas a nuestra .zshrc. ehorabuena !!!!

Felicitaciones !!! ahora tenemos pyenv

```bash
pyenv  
pyenv 2.0.1-5-gbbcecc75
Usage: pyenv <command> [<args>]

Some useful pyenv commands are:
   commands    List all available pyenv commands
   exec        Run an executable with the selected Python version
   global      Set or show the global Python version(s)
   help        Display help for a command
   hooks       List hook scripts for a given pyenv command
   init        Configure the shell environment for pyenv
   install     Install a Python version using python-build
   local       Set or show the local application-specific Python version(s)
   prefix      Display prefix for a Python version
   rehash      Rehash pyenv shims (run this after installing executables)
   root        Display the root directory where versions and shims are kept
   shell       Set or show the shell-specific Python version
   shims       List existing pyenv shims
   uninstall   Uninstall a specific Python version
   --version   Display the version of pyenv
   version     Show the current Python version(s) and its origin
   version-file   Detect the file that sets the current pyenv version
   version-name   Show the current Python version
   version-origin   Explain how the current Python version is set
   versions    List all Python versions available to pyenv
   whence      List all Python versions that contain the given executable
   which       Display the full path to an executable

See `pyenv help <command>' for information on a specific command.
For full documentation, see: https://github.com/pyenv/pyenv#readme
```



Procedemos a combinar el pyenv con un una herramienta la cual es un pluggin para pyenv

https://github.com/pyenv/pyenv-virtualenv


```bash
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```

```bash
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

Ahora vemos que version de pyenv tengo instalada 

```bash
➜  ~ pyenv version
system (set by /home/kali/.pyenv/version)
```

```
pyenv install --list
```

```bash
pyenv virtualenv 3.8.10 HTB
```

