# BASIC NET WORKING TOOLS

La red es y siempre será el escenario más sexy para un hacker. Un atacante puede hacer casi cualquier cosa con un simple acceso a la red, como escanear hosts, inyectar paquetes, husmear datos y explotar hosts de forma remota. Pero si te has abierto camino hasta las profundidades de un objetivo empresarial, puedes encontrarte en un pequeño dilema: no tienes herramientas para ejecutar ataques de red. No hay netcat. Sin Wireshark. Ni compilador, ni medios para instalar uno. Sin embargo, te sorprenderá encontrar que en muchos casos, tendrás una instalación de Python. Así que ahí es donde empezaremos

Este capítulo te dará algunos fundamentos sobre la creación de redes en Python usando el módulo socket (La documentación completa sobre sockets puede encontrarse aquí: [https://docs.python.org/3/library/socket.html](https://docs.python.org/3/library/socket.html)

En el camino, construiremos clientes, servidores y un proxy TCP. Luego los convertiremos en nuestro propio netcat, completo con un shell de comandos. Este capítulo es la base para los capítulos siguientes, en los que construiremos una herramienta de descubrimiento de hosts, implementaremos sniffers multiplataforma y crearemos un marco de trabajo para troyanos remotos. Empecemos

## Red Python en un párrafo

Los programadores tienen un número de herramientas de terceros para crear servidores y clientes en red en Python, pero el módulo central para todas esas herramientas es socket. Este módulo expone todas las piezas necesarias para escribir rápidamente clientes y servidores del Protocolo de Control de Transmisión (TCP) y del Protocolo de Datagramas de Usuario (UDP), utilizar sockets en bruto, etc. Para los propósitos de irrumpir o mantener el acceso a las máquinas de destino, este módulo es todo lo que realmente necesitas. Comencemos creando algunos clientes y servidores simples, los dos scripts de red más comunes que escribirás.

## TCP Client

En innumerables ocasiones, durante las pruebas de penetración, nosotros (los autores) hemos necesitado montar un cliente TCP para probar servicios, enviar datos basura, hacer fuzz, o realizar cualquier otra tarea. Si estás trabajando dentro de los confines de grandes entornos empresariales, no tendrás el lujo de usar herramientas de red o compiladores, y a veces incluso te faltará lo más básico, como la capacidad de copiar/pegar o conectarse a Internet. Aquí es donde la capacidad de crear rápidamente un cliente TCP es extremadamente útil. Pero basta de cháchara, pongámonos a codificar. Aquí está un simple cliente TCP:

```python
import socket

target_host = "localhost"
target_port = 80

# create a socket object
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# connect the client
client.connect((target_host, target_port))

# send some data
client.send(b"GET / HTTP/1.1\r\nHost: google.com\r\n\r\n")

# receive data
response = client.recv(4096)

client.close()

print(response)
```

Primero creamos un objeto socket con los parámetros AF_INET y SOCK_STREAM

1. El parámetro AF_INET indica que usaremos una dirección IPv4 estándar o un nombre de host, y SOCK_STREAM indica que será un cliente TCP. A continuación, conectamos el cliente al servidor.
2. y enviarle algunos datos en forma de bytes.
3. El último paso es recibir algunos datos de vuelta e imprimir la respuesta.
4. y luego cerrar el socket. Esta es la forma más simple de un cliente TCP, pero es la que escribirás más a menudo.

Este fragmento de código hace algunas suposiciones serias sobre los sockets que definitivamente quieres conocer. La primera suposición es que nuestra conexión siempre tendrá éxito, y la segunda es que el servidor espera que enviemos los datos primero (algunos servidores esperan enviarle los datos a usted primero y esperar su respuesta). Nuestra tercera suposición es que el servidor siempre nos devolverá los datos a tiempo. Hacemos estas suposiciones en gran medida por simplicidad. Mientras que los programadores tienen opiniones variadas sobre cómo tratar con sockets bloqueantes, manejo de excepciones en sockets, y similares, es bastante raro que los pentesters construyan estas sutilezas en sus herramientas rápidas y sucias para el trabajo de reconocimiento o explotación, así que las omitiremos en este capítulo.

