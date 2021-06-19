# Como Obterner una Shell Interactiva

## Con Python

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

```
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/
```

```
export TERM=xterm-256color
```

```
alias ll='ls -lsaht --color=auto'
```

## Pasos a seguir:

```
script /dev/null -c bash
```

```
CTRL + Z
```

```
stty raw -echo ; fg
```

```
export TERM=xterm
```

```
export SHELL=bash
```
