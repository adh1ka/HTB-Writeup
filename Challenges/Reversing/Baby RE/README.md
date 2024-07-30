CTF Name           : HackTheBox Challenges

Challenge category : Reversing

Challenge Name     : Baby RE

Challenge points   : 0 Points

Challenge desc     : Show us your basic skills! (P.S. There are 4 ways to solve this, are you willing to try them all?)


Langsung saja kita kerjakan, yang pertama kita periksa terlebih dahulu type filenya dengan command ‘file’ seperti berikut.
```
$ file baby  

baby: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=25adc53b89f781335a27bf1b81f5c4cb74581022, for GNU/Linux 3.2.0, not stripped
```

File tersebut adalah ELF64 Executable, selanjutnya kita coba lakukan command strings untuk melihat string pada program.

```
$ strings baby

/lib64/ld-linux-x86-64.so.2  
mgUa  
libc.so.6  
puts  
stdin  
fgets  
__cxa_finalize  
strcmp  
__libc_start_main  
GLIBC_2.2.5  
_ITM_deregisterTMCloneTable  
__gmon_start__  
_ITM_registerTMCloneTable  
u/UH  
HTB{B4BYH  
_R3V_TH4H  
TS_Ef  
[]A\A]A^A_
Dont run `strings` on this challenge, that is not the way!!!!  
Insert key:    
abcde122313  
Try again later.  
;*3$"  
GCC: (Debian 9.2.1-8) 9.2.1 20190909
```

Terlihat bahwa terdapat potongan flag, program meminta input dan dibawahnya terdapat string ‘abcde122313’, mari kita coba inputkan.

```
$ chmod +x baby
$ ./baby
Insert key:    
abcde122313  
HTB{B4BY_R3V_TH4TS_EZ}
```

Flag : HTB{B4BY_R3V_TH4TS_EZ}

Semoga Bermanfaat:)

For more write-ups, visit my [Medium](https://medium.com/@adh1ka) and [personal blog](https://root.ce.student.pens.ac.id).
