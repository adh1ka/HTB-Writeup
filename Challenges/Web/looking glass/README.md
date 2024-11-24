CTF Name           : HackTheBox Challenges

Challenge category : Web

Challenge Name     : looking glass

Challenge points   : 0 Points

Challenge desc     : We've built the most secure networking tool in the market, come and check it out!


Di challenge ini, kita diberikan web dengan tampilan seperti berikut.

![Pasted image 20240802012633](https://github.com/user-attachments/assets/1a9dafb1-72e8-43a8-a931-80461fed59c6)

Ada yang menarik dari web ini, yaitu titlenya ‘rce’. RCE adalah Remote Code Execution yang merupakan sebuah kerentanan dimana memungkinkan penyerang untuk mengirimkan kode program yang dapat dieksekusi oleh sistem dari jarak jauh. Bacaan tentang RCE bisa di [Cloudflare](https://www.cloudflare.com/learning/security/what-is-remote-code-execution/#:~:text=A%20remote%20code%20execution%20(RCE)%20attack%20is%20one%20where%20an,malware%20or%20stealing%20sensitive%20data.). 

Di web page di atas, secara default form yang bisa kita manipulasi adalah form IP Address, di form ini tidak terdapat sanitasi ataupun filtering terhadap input. Berbeda dari 2 form lain yang secara default berbentuk dropdown.

Untuk pengujian apakah terdapat vulnerability RCE pada form ini bisa dilakukan dengan web browser biasa ataupun tools seperti burp suite. Kita coba uji dengan menambahkan titik koma (;) setelah IP Address, lalu menambahkan command yang kita mau. Mengapa menambahkan titik koma? karena dalam pemrograman, titik koma berfungsi untuk memisahkan kode program/perintah untuk dieksekusi.

![Pasted image 20240802014201](https://github.com/user-attachments/assets/ef527a51-bf77-4039-96de-9b5cfb6409aa)


Contohnya seperti gambar di atas, saya mencoba menambahkan command ‘ls’ untuk melihat list dari isi direktorinya.

![Pasted image 20240802014317](https://github.com/user-attachments/assets/d7bb39ec-2bf1-467a-856b-76b27accdea5)


Ternyata bisa, untuk selanjutnya langsung saja kita cari flagnya dengan `; cat /flag*`. 

Dan voila kita dapatkan flagnya. 

![image](https://github.com/user-attachments/assets/b4557456-5397-4093-9dd9-986258cdbfff)



Terima Kasih Semoga Membantu :)
