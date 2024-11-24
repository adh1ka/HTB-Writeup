
Pada challenge ini, kita diberikan 2 buah file, yaitu key.pub dan flag.enc. Isi kedua file tersebut adalah sebagai berikut.

```
$ cat key.pub
-----BEGIN PUBLIC KEY-----  
MIIBHzANBgkqhkiG9w0BAQEFAAOCAQwAMIIBBwKBgQMwO3kPsUnaNAbUlaubn7ip  
4pNEXjvUOxjvLwUhtybr6Ng4undLtSQPCPf7ygoUKh1KYeqXMpTmhKjRos3xioTy  
23CZuOl3WIsLiRKSVYyqBc9d8rxjNMXuUIOiNO38ealcR4p44zfHI66INPuKmTG3  
RQP/6p5hv1PYcWmErEeDewKBgGEXxgRIsTlFGrW2C2JXoSvakMCWD60eAH0W2PpD  
qlqqOFD8JA5UFK0roQkOjhLWSVu8c6DLpWJQQlXHPqP702qIg/gx2o0bm4EzrCEJ  
4gYo6Ax+U7q6TOWhQpiBHnC0ojE8kUoqMhfALpUaruTJ6zmj8IA1e1M6bMqVF8sr  
lb/N  
-----END PUBLIC KEY-----
```

```
$ cat flag.enc
�_�vc[��~�kZ�1�Ĩ�4�I�9V�ֿ�^G���(�+3Lu"�T$���F0�VP�־j@������|j▒�������{¾�,�����YE������Xx��,��c�N&Hl2�Ӎ��[o��]0;
```

Kita memiliki sebuah public key dan flag yang terenkripsi, kita memerlukan private key untuk dapat mendekripsi flagnya. Setelah saya searching, saya menemukan adanya tools yang dapat membantu untuk menyelesaikan challenge ini. Nama toolsnya adalah [[RsaCtfTool]].  Tools ini dapat melakukan attack ke publickey untuk mendapatkan private keynya.

Langsung saja kita kerjakan

```
$ ./RsaCtfTool.py --publickey key.pub --decryptfile flag.enc

[*] Attack success with pastctfprimes method !  
[+] Total time elapsed min,max,avg: 0.0006/60.0226/15.2174 sec.  
  
Results for key.pub:  
  
Decrypted data :  
HEX : 0x000221cfb29883b06f409a679a58a4e97b446e28b244bbcd0687d178a8ab8722bf86da06a62e042c892d2921b336571e9ff7ac9d89ba90512bac4cfb8d7e4a3901bbccf5dfac01b27bddd35f1ca55344a75943df9a18eadb344cf7cf55fa0baa7005bfe32f41004854427b73316d706c335f  
5769336e3372735f34747434636b7d  
INT (big endian) : 1497194306832430076266314478305730170974165912795150306640063107539292495904192020114449824357438113183764256783752233913408135242464239912689425668318419718061442061010640167802145162377597484106658670422900749326253  
337728846324798012274989739031662527650589811318528908253458824763561374522387177140349821  
INT (little endian) : 2254657426622530096812385770472119185867159328797291996561957267591863625746440208264287067765757904480550182571974498195360963074339690939490672121949601983062245177059054965371647685607784964448707611049502095461  
7170743371827481017047908786316114794508942268154434710618690751442928771926238749045133355844096  
STR : b'\x00\x02!\xcf\xb2\x98\x83\xb0o@\x9ag\x9aX\xa4\xe9{Dn(\xb2D\xbb\xcd\x06\x87\xd1x\xa8\xab\x87"\xbf\x86\xda\x06\xa6.\x04,\x89-)!\xb36W\x1e\x9f\xf7\xac\x9d\x89\xba\x90Q+\xacL\xfb\x8d~J9\x01\xbb\xcc\xf5\xdf\xac\x01\xb2{\xdd\xd3_\x1c\  
xa5SD\xa7YC\xdf\x9a\x18\xea\xdb4L\xf7\xcfU\xfa\x0b\xaap\x05\xbf\xe3/A\x00HTB{sxxxxxxx}'  
  
PKCS#1.5 padding decoded!  
HEX : 0x004854427b73316d706c335f5769336e3372735f34747434636b7d  
INT (big endian) : 116228445871869252378692588205079217110932931184359462733572989  
INT (little endian) : 51594582506285564025554597946778804341308607376857173453085886464  
utf-8 : HTB{xxxxxxxx}  
STR : b'\x00HTB{sxxxxxxxx}'
```

