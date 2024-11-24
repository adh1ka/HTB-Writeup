
![Pasted image 20240801023754](https://github.com/user-attachments/assets/a4b147fa-8e16-46bc-b433-3e2a0a5ea8e3)

CTF Name           : HackTheBox Challenges

Challenge category : Reversing

Challenge Name     : baby Crypt

Challenge points   : 0 Points

Challenge desc     : Give me the key and take what's yours.

Yang pertama kita lakukan adalah melihat filetypenya terlebih dahulu dengan command ‘file’.

```
$ file baby_crypt  

baby_crypt: ELF 64-bit LSB pie executable, x86-64, version 1 (SYS  
V), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,  
BuildID[sha1]=24af7e68eab982022ea63c1828813c3bfa671b51, for GNU/L  
inux 3.2.0, not stripped
```

Kita coba jalankan programnya.

```
$ chmod +x baby_crypt

$ ltrace ./baby_crypt
Give me the key and I'll give you the flag: asasas  
^Tm;&d'eceprI!oI+"nsd7>  
+++ exited (status 0) +++

$ ltrace ./baby_crypt    
Give me the key and I'll give you the flag: advwaawgwa  
^  
+++ exited (status 0) +++
```

Ternyata, programnya meminta input, dan setiap input yang berbeda, maka outputnya berbeda juga. Kita lihat dulu decompilenya. Berikut adalah main functionnya.

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+4h] [rbp-3Ch]
  char *s; // [rsp+8h] [rbp-38h]
  __int64 v6[3]; // [rsp+10h] [rbp-30h] BYREF
  __int16 v7; // [rsp+28h] [rbp-18h]
  unsigned __int64 v8; // [rsp+38h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  printf("Give me the key and I'll give you the flag: ");
  s = (char *)malloc(4uLL);
  fgets(s, 4, _bss_start);
  v6[0] = 0x6F0547480C35643FLL;
  v6[1] = 0x28130304026F0446LL;
  v6[2] = 0x5000F4358280E52LL;
  v7 = 19798;
  for ( i = 0; i <= 25; ++i )
    *((_BYTE *)v6 + i) ^= s[i % 3];
  printf("%.26s\n", (const char *)v6);
  return 0;
}
```

Dari hasil decompile main function di atas dapat kita lihat bahwa program ini meminta input key, lalu mengambil 3 karakter pertama dari key, lalu melakukan perulangan for loop yang meng-XOR antara 3 karakter key tadi dengan variabel v6. Variabel v6 ini kemungkinan adalah flagnya. 

Sebelumnya saya beri penjelasan sederhana tentang fungsi xor. XOR (^) adalah salah satu gerbang logika yang mengembalikan nilai 1 apabila setiap bit berbeda, dan mengembalikan 0 apabila bitnya sama. 
Di dalam XOR, apabila A XOR B = C maka B XOR C = A.

Flag HTB{} adalah output apabila kita memberikan input dengan benar, maka kita bisa menginputkan ‘HTB’ ke program untuk mengetahui apa keynya sesuai rumus XOR di atas.

```
$ ./baby_crypt  
Give me the key and I'll give you the flag: HTB  
w0wDM;L;@LWQ`L`  
              GTG
```

Sudah kita dapatkan keynya yaitu output di atas, berhubung program tadi mengambil key dari 3 karakter pertama dari input, maka kita bisa menginputkan 3 karakter pertama di atas yaitu ‘w0w’.

```
$ ./baby_crypt  
Give me the key and I'll give you the flag: w0w  
HTB{xxxxxxxxxx!}
```

Terima Kasih Semoga Membantu :)


For more write-ups, visit my [Medium](https://medium.com/@adh1ka) and [personal blog](https://root.ce.student.pens.ac.id).
