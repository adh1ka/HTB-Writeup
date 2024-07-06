![Pasted image 20240707015502](https://github.com/adh1ka/HTB-Writeup/assets/135927661/64e2bb89-c7a0-4abf-a86a-c80089d496ba)

CTF Name		: HackTheBox Challenges

Challenge category	: Mobile

Challenge Name		: Cryptohorrific

Challenge points	: 40 Points

Challenge desc		: Secure coding is the keystone of the application security!

https://app.hackthebox.com/challenges/Cryptohorrific
![Pasted image 20240707020157](https://github.com/adh1ka/HTB-Writeup/assets/135927661/1030d82c-ddc1-4ccc-a3e8-581758d3fd5e)

Langsung saja kita kerjakan, berikut adalah isi challengenya.
![Pasted image 20240707020557](https://github.com/adh1ka/HTB-Writeup/assets/135927661/44614572-fd61-421b-b900-e9a7994259b0)

``` 
$ file *
Base.lproj:          directory  
challenge.plist:     Apple binary property list  
_CodeSignature:      directory  
hackthebox:          Mach-O 64-bit x86_64 executable, flags:<NOUNDEFS|DYLDLINK|TWOLEVEL|PIE>  
htb-company.png:     PNG image data (CgBI), 190 x 90, 8-bit/color RGBA, non-interlaced  
Info.plist:          Apple binary property list  
PkgInfo:             ASCII text, with no line terminators
```

Dari hasil command file, dapat kita tarik informasi bahwa challenge ini tentang aplikasi Apple.

Yang pertama kita ubah challenge.plist menjadi xml agar mudah dibaca, berikut commandnya.

```
plistutil -i challenge.plist -o challenge.plist.xml
```

Berikut adalah isi challenge.plist dalam bentuk XML.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<array>
	<dict>
		<key>flag</key>
				<string>Tq+CWzQS0wYzs2rJ+GNrPLP6qekDbwze6fIeRRwBK2WXHOhba7WR2OGNUFKoAvyW7njTCMlQzlwIRdJvaP2iYQ==</string>
		<key>id</key>
		<string>123</string>
		<key>title</key>
		<string>HackTheBoxIsCool</string>
	</dict>
</array>
</plist>
```

Dari XML diatas, dapat kita lihat bahwa terdapat flag dalam betuk base64 yang masih terenkripsi, setelah itu kita lakukan disassemble aplikasi ‘hackthebox’, saya akan menggunakan IDA.

Berikut isi pseudecode main function Objective-C-nya.

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  objc_class *v3; // rax
  NSString *v4; // rax
  NSString *v6; // [rsp+0h] [rbp-30h]
  void *context; // [rsp+8h] [rbp-28h]
  int v8; // [rsp+2Ch] [rbp-4h]

  context = objc_autoreleasePoolPush();
  v3 = (objc_class *)+[AppDelegate class](&OBJC_CLASS___AppDelegate, "class");
  v4 = NSStringFromClass(v3);
  v6 = objc_retainAutoreleasedReturnValue(v4);
  v8 = UIApplicationMain((unsigned int)argc, argv, 0LL, v6);
  objc_release(v6);
  objc_autoreleasePoolPop(context);
  return v8;
}
```

Sekilas, dari main function di atas tidak terlihat sesuatu yang mencurigakan. 
![Pasted image 20240707022102](https://github.com/adh1ka/HTB-Writeup/assets/135927661/340879a7-8782-4035-a394-12729a1fd478)

Pada Function name, terlihat ViewController SecretManager:key:iv:data, yang bisa menjadi petunjuk selanjutnya. Berdasarkan SecretManager ini, terlihat bahwa metode enkripsinya menggunakan AES.

Namun, di dalam ViewController SecretManager tidak terlihat sesuatu yang mencurigakan juga. Kita coba lihat function viewDidLoad.

![Pasted image 20240707022741](https://github.com/adh1ka/HTB-Writeup/assets/135927661/48654300-e26e-40f2-a8ab-08f37048bfca)


Kita mendapatkan key dan iv-nya.

```
 key : !A%D*G-KaPdSgVkY
 iv  : QfTjWnZq4t7w!z%C
```

Setelah itu kita tinggal lakukan dekripsi base64 tadi dengan key ini.
Bisa dilakukan di [sini](https://www.devglan.com/online-tools/aes-encryption-decryption) .

![Pasted image 20240707023103](https://github.com/adh1ka/HTB-Writeup/assets/135927661/a0e95a96-c6dd-41ad-9573-e4242df9bcde)


Terima Kasih :)
