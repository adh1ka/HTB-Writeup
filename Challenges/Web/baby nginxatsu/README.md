CTF Name           : HackTheBox Challenges

Challenge category : Web

Challenge Name     : baby nginxatsu

Challenge points   : 0 Points

Challenge desc     : Can you find a way to login as the administrator of the website and free nginxatsu?

Diberikan sebuah web yang terdapat form login.

![Pasted image 20240802214930](https://github.com/user-attachments/assets/1239e91c-3a20-4bc0-9d55-2412115c9fdc)


Langsung saja buat akun baru lalu login dengan akunnya. Berikut adalah tampilan ketika sudah berhasil login. 

![Pasted image 20240802215030](https://github.com/user-attachments/assets/a7697326-14b3-4ed5-849f-781ef9fa1ad6)


Yang menarik di sini adalah di tab Routes, di dalam kolom Location tercantum /storage yang berarti terdapat direktori /storage pada web ini, mari kita coba akses. Berikut adalah isinya. 

![Pasted image 20240802215230](https://github.com/user-attachments/assets/d6bbbdee-5feb-4f00-a0a9-9a775bf68c55)


Ada yang menarik di sini, yaitu di bagian bawah sendiri terdapat sebuah file yang berbeda, yaitu berekstensi .tar.gz. Mari kita download setelah itu kita ekstrak. Ternyata isinya adalah database.sqlite, mari kita buka dengan command seperti di bawah.

```
$ sqlite3 database.sqlite

SQLite version 3.46.0 2024-05-23 13:25:27  
Enter ".help" for usage hints.

sqlite> SELECT * FROM users;  

1|jr|nginxatsu-adm-01@makelarid.es|e7816e9a10590b1e33b87ec2fa65e6cd|YWKuXNJFtO8MgAJacKxoxgfDElJ7XrqowZonYkEfZXQK0inc5De05ut04FjepWRoSxNG||2024-08-02 14:23:00|2024-08-02 14:23:00  
2|Giovann1|nginxatsu-giv@makelarid.es|d73ce8d221aa4b84911db2d300041cdc|EUaaFTdJpyGg3xCHkjCuaoB3JHWWxFXCvn3d9Gs320pz7acuYhBHBn26BoRY4KeoA107||2024-08-02 14:23:00|2024-08-02 14:23:00  
3|me0wth|nginxatsu-me0wth@makelarid.es|7cc1ba923517c025907f3394f876090f|0rgKCrT0fFvirbnyggNa2jGqiqbQn7T9amJ6CFIpIki6ez89p8dP85wq3NQlonyWjaB0||2024-08-02 14:23:00|2024-08-02 14:23:00
```

Terlihat terdapat 3 data user, yang nomor 1 merupakan administrator dan nampaknya passwordnya masih terenkripsi. Kita coba identifikasi enkripsinya dengan [dCode](https://www.dcode.fr/cipher-identifier).


![Pasted image 20240802220926](https://github.com/user-attachments/assets/b63e18fa-245b-40b2-983d-9dcd9c09bee7)

Dcode mengenali ini sebagai MD5 Hash, langsung aja decrypt dengan cara klik ‘MD5’ di bagian Results.

![Pasted image 20240802221102](https://github.com/user-attachments/assets/53fa01ef-4ab2-40d1-affd-4ce8b0980340)


Kita dapatkan passwordnya yaitu ‘adminadmin1’, setelah itu kita login di web awal tadi dengan data admin yang terdapat pada database.sqlite tadi, yaitu email : nginxatsu-adm-01@makelarid.es dan password : adminadmin1.

![Pasted image 20240802221247](https://github.com/user-attachments/assets/49ec3201-ae27-4831-9781-fb4b17447254)


Voila kita dapatkan flagnya.

Flag : HTB{ng1ngx_r34lly_b3_sp1ll1ng_my_w3ll_h1dd3n_s3cr3ts??}

Terima Kasih Semoga Membantu:)
