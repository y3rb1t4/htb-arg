# El Menu de Kali_linux:

<p>El menú de Kali Linux incluye enlaces categóricos para muchas de las herramientas presentes en la distribución. Esta estructura ayuda a aclarar la función principal de cada herramienta, así como el contexto para su uso.</p>


![[Pasted image 20210614044559.png]]


## Primeros pasos en kali

Kali Linux se adhiere al estándar de jerarquía del sistema de archivos (FHS),35 que proporciona una disposición familiar y universal para todos los usuarios de Linux. Los directorios que encontrarás más útiles son:

```bash


```

## Primeros Comandos en Kali Linux

man ls

man -

### apropos particion






updates on kali:

wpscan -update

searchploit -u

nmap --script-updatedb

joomscan --update

Other stuff to do on kali:

to take a screenshot:

ctrl + shift + printscreen

alt + printscreen

go:(kali>2020)

installation:

apt-get install golang

append these in ~/.zshrc:

export GOPATH=$HOME/golang

export GOROOT=/usr/lib/go
export GOBIN=$GOPATH/bin

export PATH=$PATH:$GOPATH

export PATH=$PATH:$GOROOT/bin

source ~/.zshrc # it will run the changes made above

explanation:

we use "~/.zshrc" as we are using zsh (not bash, it has ~/.bashrc). ~/.zshrc is
responsible for maintaining shell environment

export command is used to make env variables

other tools to install:

install chrome

install opera

install sublime

install evernote

install terminator

install nessus

install wfuzz

install Seclists

terminal shortcuts:

ctrl + shift + v => paste

ctrl + shift + t => new tab

ctrl + shift + n => new window

ctrl + shift + w => close tab

ctrl + shift + q => close window

f11 => full screen

ctrl + o => normal size

ctrl + pageup => previous tab

ctrl + pagedown => next tab
