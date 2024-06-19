# Writeup-BCACTF5.0

Here's a GitHub README template based on the analysis and write-up of the BCA CTF 5.0 challenges:

---

# BCA CTF 5.0 Write-Up

## Introduction
Welcome to the BCA CTF 5.0 Write-Up! This document details the solutions for the challenges presented in the Capture The Flag (CTF) competition.

## Table of Contents
1. [Inaccessible (binex)](#inaccessible-binex)
2. [Tic-Tac-Toe (webex)](#tic-tac-toe-webex)
3. [XOR (crypto)](#xor-crypto)
4. [NOTflag (crypto)](#notflag-crypto)
5. [Time Skip (crypto)](#time-skip-crypto)
6. [Vinegar Times 3 (crypto)](#vinegar-times-3-crypto)
7. [Chalkboard Gag (forensic)](#chalkboard-gag-forensic)
8. [Sea Scavenger (forensic)](#sea-scavenger-forensic)

## Inaccessible (binex)
### Deskripsi
Melakukan analisis menggunakkan decompiler online, kami menggunakkan [Decompiler Explorer](https://dogbolt.org/) lalu menemukan program hasil compile dalam file tersebut

### dekompile menggunakkan Ghidra
```c
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
```c
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
```bin 
0040045d 48 c7 c7        MOV        RDI,main
                 b8 06 40 00
```

### Menjalankan program
```bin 
$ ./chall_mod
```

dan didapati flagnya **bcactf{W0w_Y0u_m4d3_iT_b810c453a9ac9}**

## Time Skip (crypto)
### Deskripsi
Melakukan analisis dengan mudah via bantuan chatgpt untuk menentukan algoritma kiptografi yang digunakan. Sebenarnya kita dapat menemukan langsung flagnya dikarenakan diberikan langsung pada ciphertextnya.

### Flag (ciphertext)
``` 
heyguysimkindoflostprobablynotgoingtosurvivemuchlongertobehonestbutanywaystheflagisbcactf{5c7t4l3_h15t04y_qe829xl1}pleasesendhelpimeanbythetimeyouseethisiveprobablybeendeadforthousandsofyearsohwellseeyoulaterisupposebyee
```

<br>Didapati algoritma yang digunakan adalah algoritma Scytale, kami menggunakkan [online decoder](https://www.dcode.fr/scytale-cipher) 
didapati flagnya **bcactf{5c7t4l3_h15t04y_qe829xl1}**

## Tic-Tac-Toe (webex)
### Description
Win the tic-tac-toe game using Burp Suite.

### Solution
Intercept WebSocket communication and modify the game state.

### Flag
```
bcactf{7h3_m4st3r_0f_t1ct4ct0e_678d52c8}
```

## XOR (crypto)
### Description
Decrypt XOR encrypted flag using the given key.

### Solution
Used the ASCII key `ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7` to decrypt.

### Flag
```
bcactf{SYMmE7ric_eNcrYP710N_4WD0f229}
```

## NOTflag (crypto)
### Description
Decode the Base64 encrypted flag.

### Solution
Used a Base64 decoder.

### Flag
```
bcactf{7hIs_1s_7h3_fla9}
```

## Time Skip (crypto)
### Description
Decrypt the Scytale cipher text.

### Solution
Used an online Scytale cipher decryption tool.

### Flag
```
bcactf{5c7t4l3_h15t04y_qe829xl1}
```

## Vinegar Times 3 (crypto)
### Description
Decrypt the Vigenere cipher using multiple keys.

### Solution
Sequential decryption with keys `vinegar`, `redwine`, and `balsamic`.

### Flag
```
bcactf{add_to_salad_yummy}
```

## Chalkboard Gag (forensic)
### Description
Find differences in repeated phrases.

### Solution
Custom Python script to extract deviations.

### Flag
```
bcactf{BaRT_W0U1D_B3_PR0uD}
```

## Sea Scavenger (forensic)
### Description
Find the specific ID from HTML comments and access the URL.

### Solution
Used cURL to fetch the URL with the derived ID.

### Flag
```
bcactf{R3gex_WH1z_54dfa9cdba13}
```

## Conclusion
This document provides the detailed solutions for each challenge in BCA CTF 5.0. Each section outlines the challenge, the approach used, and the final flag obtained.

---

Feel free to copy this into your `README.md` file on GitHub.
