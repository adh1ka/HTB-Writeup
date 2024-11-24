CTF Name           : HackTheBox Challenges

Challenge category : Reversing

Challenge Name     : You Cant C Me

Challenge points   : 0 Points

Challenge desc     : Can you see me?

Langsung saja kita kerjakan, yang pertama kita lihat filetypenya, berikut adalah commandnya.

```
$ file auth  

auth: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, stripped
```

Selanjutnya kita lihat stringnya.

```
$ strings auth
```

Tidak terlihat sesuatu yang mencurigakan, selanjutnya kita coba tracing dengan ltrace. Program akan meminta inputan, kita coba input sembarang string.

```
$ ltrace ./auth

printf("Welcome!\n"Welcome!  
)                   = 9  
malloc(21)                             = 0x2d4906b0  
fgets(asasasas  
"asasasas\n", 21, 0x7f60d188a8e0) = 0x2d4906b0  
strcmp("wh00ps!_y0u_d1d_c_m3", "asasasas\n") = 22  
printf("I said, you can't c me!\n"I said, you can't c me!  
)    = 24  
+++ exited (status 0) +++
```

Terlihat bahwa program melakukan pengecekan input dengan string `wh00ps!_y0u_d1d_c_m3`. Selanjutnya kita jalankan program lalu masukkan string di atas sebagai inputnya.

```
$ ./auth    
Welcome!  
wh00ps!_y0u_d1d_c_m3  
HTB{xxxxxxx}
```
