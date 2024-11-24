CTF Name           : HackTheBox Challenges

Challenge category : Web

Challenge Name     : sanitize

Challenge points   : 0 Points

Challenge desc     : Can you escape the query context and log in as admin at my super secure login page?

![Pasted image 20240802020456](https://github.com/user-attachments/assets/79537925-4c40-4089-8834-c5ab5cf85166)


Dari title webnya, kita mendapatkan clue bahwa web ini rentan terhadap SQL Injection, SQL Injection adalah kerentanan di mana input tidak disanitasi/difilter dengan benar sehingga penyerang dapat melakukan manipulasi untuk mengakses informasi. Bacaan tentang SQL Injection bisa di [sini](https://www.w3schools.com/sql/sql_injection.asp).

Mari kita coba menguji kerentanannya, saya akan mengikuti artikel dari [TCM Security](https://tcm-sec.com/avoid-or-1-equals-1-in-sql-injections/).


![Pasted image 20240802021525](https://github.com/user-attachments/assets/40f38f7d-a8a3-4ccd-b9af-5c95a82f2f73)


Ketika saya masukkan ‘admin’ pada username dan passwordnya, keluar tampilan yang menunjukkan SQL syntax dari form loginnya. 
```
select * from users where username = 'admin' AND password = 'admin';
```

Jika kita memasukkan username sebagai `' OR 1=1 -- -` maka berikut adalah SQL syntaxnya.

```
select * from users where username = '' OR 1=1 -- - AND password = 'admin';
```

OR 1=1 akan selalu menghasilkan nilai true, dan —- -adalah comment dalam SQL yang berarti apabila kita memasukkan password, password tersebut tidak akan diproses oleh program karena terbaca sebagai comment.

Berikut adalah outputnya.

![image](https://github.com/user-attachments/assets/27a80e1d-04ba-45d3-ad15-f95a0ff80bdd)


