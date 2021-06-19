# Unobtainium IP= 10.10.10.235


## Escaneo de Puertos

```
 nmap -sS --min-rate 5000 -T5 -n -Pn -p- 10.10.10.235             
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-09 23:48 EDT
Warning: 10.10.10.235 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.10.235
Host is up (0.23s latency).
Not shown: 65526 closed ports
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
358/tcp   filtered shrinkwrap
2379/tcp  open     etcd-client
2380/tcp  open     etcd-server
8443/tcp  open     https-alt
10250/tcp open     unknown
10256/tcp open     unknown
31337/tcp open     Elite

Nmap done: 1 IP address (1 host up) scanned in 19.33 seconds

```

```
