https://0xdf.gitlab.io/2021/03/20/htb-crossfit.html

# Machine Crossfit 10.10.10.208

<p>CrossFit es una caja Linux de dificultad insana con un servidor Apache que aloja el sitio web de un gimnasio ficticio "CrossFit Club". El sitio web hace uso de un mecanismo de prevención de XSS que registra las direcciones IP y los agentes de usuario de los intentos de XSS detectados. El registro se muestra en una página web que es visitada periódicamente por un administrador, y puede ser utilizado como fuente de XSS ciego. CORS se utiliza para enumerar los subdominios que aceptan recursos de origen cruzado mediante el envío de cabeceras de origen y la búsqueda de cabeceras de respuesta Access-Control-Allow-Origin. Esto lleva a la identificación de un host virtual que permite la creación de usuarios FTP que tienen permiso para subir archivos a un directorio web.</p>

<p>Como no se puede acceder a este host virtual de forma remota, se explota la vulnerabilidad XSS para realizar un ataque CSRF. Esto requiere leer un token anti-CSRF y enviarlo junto con la peticiónPOST que estamos enviando. Al encadenar XSS y CSRF, se puede añadir un usuario FTP y luego utilizarlo para subir archivos PHP maliciosos, lo que resulta en la ejecución remota de comandos como el usuario www-dat. La escalada al usuario sin privilegios hank requiere la lectura de un hash de contraseña en un archivo Ansible playbook legible por el mundo y su crackeo.</p>

<p>A continuación, se identifica y explota una vulnerabilidad de inyección de comandos, lo que nos permite movernos lateralmente hacia el usuario issac. Un segundo trabajo de cron puede ser explotado mediante ingeniería inversa de un programa C. Identificamos una primitiva de escritura arbitraria, que permite crear archivos de un formato determinado como root. Enlazando archivos arbitrarios a /root/.ssh/authorized_keys antes de que el programa se ejecute, y escribiendo una clave pública generada por el usuario en la base de datos que luego se escribirá en el archivo authorized_keys, obtenemos acceso SSH al sistema como root.</p>

## Habilidades Requeridas

<ul>
    <li>Enumeracion</li>
    <li>Conocimientos forenses</li>
    <li>Revisión del código</li>
    <li>XSS (Cross Site Scripting)</li>
    <li>CSRF (Cross-Site Request Forgery)</li>
    <li>CORS (Cross-Origin Resource Sharing)</li>
    <li>Basic Reverse Engineering</li>
</ul>

## Habilidades Aprendidas

<ul>
    <li>Blind XSS</li>
    <li>COR's (Cross-Origin Resource Sharing) Misconfiguration and Exploitation</li>
    <li>Code Review</li>
    <li>PAM</li>
    <li>Password Reuse & Cracking</li>
    <li>Code Disassembly</li>
</ul>


gym-club.crossfit.htb  

```bash
🧉
# Nmap 7.91 scan initiated Mon Jun 14 16:10:49 2021 as: nmap -sS --min-rate 5000 -T5 -n -Pn -oA allPorts -p- 10.10.10.208
Warning: 10.10.10.208 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.10.208
Host is up (0.20s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http

# Nmap done at Mon Jun 14 16:11:03 2021 -- 1 IP address (1 host up) scanned in 14.21 seconds
```




```bash
🧉
# Nmap 7.91 scan initiated Mon Jun 14 16:12:52 2021 as: nmap -sC -sV -p21,22,80 -oN targeted 10.10.10.208
Nmap scan report for 10.10.10.208
Host is up (0.19s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
| ssl-cert: Subject: commonName=*.crossfit.htb/organizationName=Cross Fit Ltd./stateOrProvinceName=NY/countryName=US
| Not valid before: 2020-04-30T19:16:46
|_Not valid after:  3991-08-16T19:16:46
|_ssl-date: TLS randomness does not represent time
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 b0:e7:5f:5f:7e:5a:4f:e8:e4:cf:f1:98:01:cb:3f:52 (RSA)
|   256 67:88:2d:20:a5:c1:a7:71:50:2b:c8:07:a4:b2:60:e5 (ECDSA)
|_  256 62:ce:a3:15:93:c8:8c:b6:8e:23:1d:66:52:f4:4f:ef (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Apache2 Debian Default Page: It works
Service Info: Host: Cross; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jun 14 16:13:12 2021 -- 1 IP address (1 host up) scanned in 19.82 seconds
```

```bash
🧉
openssl s_client -connect 10.10.10.208:21 -starttls ftp
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 C = US, ST = NY, O = Cross Fit Ltd., CN = *.crossfit.htb, emailAddress = info@gym-club.crossfit.htb
```

<p>El CN del certificado, también conocido como nombre común, se establece con el valor *.crossfit.htb , mientras que la dirección de correo electrónico asociada utiliza el subdominio gym-club.crossfit.htb. Tomamos nota de este dominio, ya que podría ser útil más adelante. Naveguemos hacia el puerto 80 utilizando un navegador para comprobar la página web en http://10.10.10.208/ . Nos encontramos con la página por defecto de Apache/Debian.</p>

Añadamos el dominio gym-club.crossfit.htb a nuestro archivo /etc/hosts y naveguemos hasta él.

```bash
🧉
echo "10.10.10.208 gym-club.crossfit.htb" >> /etc/hosts
```