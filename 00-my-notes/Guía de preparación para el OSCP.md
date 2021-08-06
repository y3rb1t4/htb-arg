# Guía de preparación para el OSCP



¿ Qué es (OSCP) ?

Requisitos del curso

Resumen del curso

Entorno de laboratorio

Examen

Preparación del examen

Consejos para realizar el examen OSCP

Recursos y sitios web recomendados.



### Requisitos del curso

Antes de decidirse a inscribirse en el curso es necesario tener alguna experiencia en las siguientes áreas:

1.Fundamentos de las redes TCP/IP

- Direccionamiento TCP/IP y Subredes 
- Comprender cómo se envía y recibe el tráfico en la red 
- Tipos de protocolos y servicios que se ejecutan en ellos.

2.Programming Languages

- Bash
- Python 
- Perl 
- Ruby

3.Operating Systems Knowledge



4.Note Taking



Resumen del curso 



Máquinas prácticas para preparar el OSCP

OSCP-Like VMs on Vulnhub  and Hackthebox: 

http://tiny.cc/OSCP_PREP

Prepare for OSCP on HTB with IppSec: 

https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA



Recursos para prepararse 

Enumeracion:

La enumeración es lo más importante que puedes hacer, cuando te encuentres que te encuentres con un muro, el 90% de las veces será porque no has hecho suficiente enumeración.

A continuación se presentan comandos que son útiles mientras estás en el laboratorio: 

Nmap

Quick TCP Scan

nmap -sC -sV -vv -oA quick target

Quick UDP Scan

nmap -sU -sV -vv -oA quick_udp target

Full TCP Scan

nmap -sC -sV -p- -vv -oA full target

Port knock

for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x target; done

Web Scanning

Gobuster: una rápida búsqueda de directorios

```bash
gobuster -u target -w /usr/share/seclists/Discovery/Web_Content/common.txt -t 80 -a Linux 
```

Obtener una TTY totalmente interactiva utilizando Python

Cuando obtengas una reverse shell

```bash
# Enter while in reverse shell 
$ python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z
# In Kali
$ stty raw -echo
$ fg
# In reverse shell 
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols>
```

