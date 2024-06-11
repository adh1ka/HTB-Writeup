
Diberikan sebuah file windows executable (.exe), setelah dibuka ternyata sebuah program yang meminta password.

![Pasted image 20240611231835](https://github.com/adh1ka/HTB-Writeup/assets/135927661/816f3f8f-9e45-4372-b985-a471220d54d1)

Pertama kita coba untuk strings terlebih dahulu.

```
strings EasyPass.exe
```
![Pasted image 20240611230928](https://github.com/adh1ka/HTB-Writeup/assets/135927661/02f4ce5c-f33a-4187-898f-20ef5e409e87)



Outputnya terlihat tidak terdapat apa-apa, selanjutnya kita coba untuk melakukan static analysis menggunakan IDA ataupun Ghidra.

Di IDA, buka file seperti biasa, setelah itu kita cari string “Password” dengan klik View → Open Subviews → String.
![Pasted image 20240611231428](https://github.com/adh1ka/HTB-Writeup/assets/135927661/79b92be2-a39a-4231-beb5-f52380552be1)


Setelah itu klik pada Good Job Congratulations agar diarahkan pada hexnya berada. Terlihat terdapat string yang mencurigakan di hex tersebut, string tersebut bertuliskan fortran! namun agak terpisah-pisah.
![Pasted image 20240611231647](https://github.com/adh1ka/HTB-Writeup/assets/135927661/3e88ac0f-0054-436f-b296-b529daf5c0df)


Kita coba untuk memasukkan string tersebut pada kolom password.

![Pasted image 20240611231919](https://github.com/adh1ka/HTB-Writeup/assets/135927661/2738c6c7-4bcf-46de-8476-258cf219348a)


Dan ternyata benar, passwordnya adalah fortran!.
