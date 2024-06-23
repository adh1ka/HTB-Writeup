Diberikan sebuah file ELF64, langsung saja kita buka dengan ida ataupun ghidra. Berikut adalah hasil decompile main function.

```
int __fastcall main(int a1, char **a2, char **a3)
{
  const char *v3; // rsi
  int result; // eax
  char v5[32]; // [rsp+10h] [rbp-40h] BYREF
  char s1[20]; // [rsp+30h] [rbp-20h] BYREF
  char *s2; // [rsp+48h] [rbp-8h]

  s2 = "SuperSeKretKey";
  qmemcpy(v5, "A]Kr=9k0=0o0;k1?k81t", 20);
  printf("* ");
  __isoc99_scanf("%20s", s1);
  printf("[%s]\n", s1);
  if ( strcmp(s1, s2) )
    exit(1);
  printf("** ");
  __isoc99_scanf("%20s", s1);
  v3 = (const char *)sub_40078D(20LL);
  result = strcmp(s1, v3);
  if ( !result )
    return sub_400978(v5);
  return result;
}
```


Dari main function di atas, dapat dilihat bahwa terdapat 2 string yaitu di variabel s2 dan v5. Program ini akan meminta input terlebih dahulu, lalu input akan dicompare dengan variabel s2, dan apabila true maka program meminta input lagi, setelah itu inputan akan dicompare lagi dengan hasil dari function sub_40078D. Berikut isi dari functionnya.

```
_BYTE *__fastcall sub_40078D(int a1)
{
  int v1; // eax
  _BYTE *v3; // [rsp+20h] [rbp-10h]
  int i; // [rsp+2Ch] [rbp-4h]

  v1 = time(0LL);
  ++dword_601074;
  srand(a1 * v1 + dword_601074);
  v3 = malloc(a1 + 1);
  if ( !v3 )
    exit(1);
  for ( i = 0; i < a1; ++i )
    v3[i] = rand() % 94 + 33;
  v3[a1] = 0;
  return v3;
}
```

Function ini menggunakan lib rand() sehingga agak susah untuk kita prediksi hasilnya, memerlukan dynamic analysis. Kita balik lagi ke main function untuk mencari cara lain. Di akhir main function, terdapat function yang dipanggil yaitu sub_400978(v5). Berikut isi dari functionnya.

```
int __fastcall sub_400978(_BYTE *a1)
{
  int v1; // eax
  int v3; // [rsp+14h] [rbp-Ch]

  v3 = 0;
  while ( *a1 != 9 )
  {
    v1 = v3++;
    if ( v1 > 19 )
      break;
    putchar((char)(*a1++ ^ 9));
  }
  return putchar(10);
}
```

Function ini melakukan XOR v5 dengan 9, mari kita coba untuk selesaikan.

```
python3
>>> var = "A]Kr=9k0=0o0;k1?k81t"
>>> flag = ''.join(chr(ord(b) ^ 9) for b in var)
>>> print(flag)
HTB{xxx}
```

