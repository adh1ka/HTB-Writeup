CTF Name           : HackTheBox Challenges

Challenge category : Web

Challenge Name     : baby auth

Challenge points   : 0 Points

Challenge desc     : Who needs session integrity these days?

Pada challenge ini, kita diberikan sebuah web page yang berisikan login form.

![Pasted image 20240802170048](https://github.com/user-attachments/assets/9efa3527-6fdf-40dd-a76a-2924dc028df2)


Yang menarik dari web ini adalah titlenya ‘broken authentication’. Broken authentication merupakan kerentanan di mana lemahnya konfigurasi ataupun implementasi dari identity and access control. Broken authentication ini memungkinkan penyerang untuk mengakses data atau informasi sensitif seperti key atau session token, informasi user, ataupun password. Bacaan tentang broken authentication bisa di [OWASP](https://owasp.org/www-project-top-ten/2017/A2_2017-Broken_Authentication) ataupun [Portswigger](https://portswigger.net/web-security/authentication).

Yang pertama kita lakukan adalah kita coba untuk membuat akun baru pada ‘Create your account →’. 
![Pasted image 20240802170628](https://github.com/user-attachments/assets/0464a097-6c3e-4d1b-b638-2d1a890ecc49)




Setelah itu masukkan username dan password, lalu login menggunakan akun yang didaftarkan.

![Pasted image 20240802170742](https://github.com/user-attachments/assets/88481e81-6c4e-4498-b26d-23d99f9a543d)



Setelah berhasil login, akan muncul page seperti di atas. Berdasarkan pesan di atas, kita terdeteksi bahwa session login kita bukan admin. 

Selanjutnya kita periksa di bagian cookies untuk memeriksa apakah ada cookies yang bisa dimanipulasi di sana. Inspect page → Application → Cookies.


![Pasted image 20240802171139](https://github.com/user-attachments/assets/d980784c-48d5-4880-83e1-86277aadc926)



Terlihat terdapat cookies PHPSESSID yang valuenya berupa ciphertext.


![Pasted image 20240802171251](https://github.com/user-attachments/assets/212b27cf-83e4-468e-8af2-57243205452b)



%3D adalah symbol samadengan ‘=’ yang terencode dengan URL encode. Di value PHPSESSID di atas, terlihat bahwa terdapat 2 %3D yang berarti terdapat 2 sama dengan. Dua sama dengan pada ciphertext bisa jadi adalah sebuah base64 encode. Untuk melakukan decode bisa kita lakukan di [Cyberchef](https://gchq.github.io/CyberChef/#recipe=URL_Decode()From_Base64('A-Za-z0-9%2B/%3D',true,false)&oeol=CR).

![Pasted image 20240802171707](https://github.com/user-attachments/assets/0f798bbd-3674-4f89-b7e6-93ab4fdcbac0)




PHPSESSID tadi berisi username yang kita gunakan untuk login. Berhubung pada web pagenya berisi ‘You are not an admin’, kita ganti username kita dengan ‘admin’, lalu base64 encodenya kita masukkan ke dalam PHPSESSID.

Dan voila kita dapatkan flagnya.


![Pasted image 20240802172035](https://github.com/user-attachments/assets/4cc3744c-7b90-4b7e-a987-d0cdc9ef66ee)



Flag : HTB{s3ss10n_1nt3grity_1s_0v3r4tt3d_4nyw4ys}

Terima Kasih Semoga Bermanfaat :)

