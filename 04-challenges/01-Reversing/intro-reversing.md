

### 02- You Cant C Me

Tenemos que descargar el archivo del challenge.




```bash
➜  Downloads ./auth
Welcome!
sdada
I said, you can't c me!
```

### que hace el comando strings?


```bash
➜  you_cant_c_me chmod +x auth
➜  you_cant_c_me ./auth
Welcome!
asd
I said, you can't c me!
```
```bash
➜  you_cant_c_me strings -n 10 auth
/lib64/ld-linux-x86-64.so.2
__libc_start_main
GLIBC_2.2.5
__gmon_start__
[]A\A]A^A_
I said, you can't c me!
this_is_the_password
Uo&kUZ'ZUYUc)
GCC: (Ubuntu 9.2.1-9ubuntu2) 9.2.1 20191008
clang version 6.0.1-11 (tags/RELEASE_601/final)
.note.ABI-tag
.gnu.version
.gnu.version_r
.eh_frame_hdr
.init_array
.fini_array
➜  you_cant_c_me ./auth
Welcome!
this_is_the_password
I said, you can't c me!
```

```bash
➜  you_cant_c_me ltrace ./auth
printf("Welcome!\n"Welcome!
)                                                                                                             = 9
malloc(21)                                                                                                                       = 0xf0d6b0
fgets(this_is_the_password
"this_is_the_password", 21, 0x7fee6b6db980)                                                                                = 0xf0d6b0
strcmp("wh00ps!_y0u_d1d_c_m3", "this_is_the_password")                                                                           = 3
printf("I said, you can't c me!\n"I said, you can't c me!
)                                 
```


