下載檔案後執行`warm -h`即可得到flag

```console
$ wget -q https://mercury.picoctf.net/static/a14be2648c73e3cda5fc8490a2f476af/warm
$ file warm
warm: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=3181a501366281ab5eba1c41e54a1f40800e3966, with debug_info, not stripped
$ chmod +x warm
$ ./warm
Hello user! Pass me a -h to learn what I can do!
$ ./warm -h
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_755f3544}
```

或者也可以用 `strings` 命令来查看執行檔中的字串

```console
$ strings warm | grep picoCTF
picoCTF{b1scu1ts_4nd_gr4vy_755f3544}
```

或者用`xxd`指令來查看執行檔的內容

```console
$ xxd warm | grep -C 3 picoCTF
00000820: 616c 6c79 2064 6f6e 2774 2064 6f20 6d75  ally don't do mu
00000830: 6368 2c20 6275 7420 4920 646f 2068 6176  ch, but I do hav
00000840: 6520 7468 6973 2066 6c61 6720 6865 7265  e this flag here
00000850: 3a20 7069 636f 4354 467b 6231 7363 7531  : picoCTF{b1scu1
00000860: 7473 5f34 6e64 5f67 7234 7679 5f37 3535  ts_4nd_gr4vy_755
00000870: 6633 3534 347d 0000 4920 646f 6e27 7420  f3544}..I don't 
00000880: 6b6e 6f77 2077 6861 7420 2725 7327 206d  know what '%s' m
```

其中`grep -C 3` 代表顯示匹配行的前後3行

或者用`hexdump`也可以做到相同的事

```console
$ hexdump -C warm | grep -C 3 picoCTF
00000820  61 6c 6c 79 20 64 6f 6e  27 74 20 64 6f 20 6d 75  |ally don't do mu|
00000830  63 68 2c 20 62 75 74 20  49 20 64 6f 20 68 61 76  |ch, but I do hav|
00000840  65 20 74 68 69 73 20 66  6c 61 67 20 68 65 72 65  |e this flag here|
00000850  3a 20 70 69 63 6f 43 54  46 7b 62 31 73 63 75 31  |: picoCTF{b1scu1|
00000860  74 73 5f 34 6e 64 5f 67  72 34 76 79 5f 37 35 35  |ts_4nd_gr4vy_755|
00000870  66 33 35 34 34 7d 00 00  49 20 64 6f 6e 27 74 20  |f3544}..I don't |
00000880  6b 6e 6f 77 20 77 68 61  74 20 27 25 73 27 20 6d  |know what '%s' m|
```

`hexdump`預設不會顯示ASCII字元，要加`-C`參數來顯示

或者直接用`grep`指令

```console
$ grep -a picoCTF warm
]��f.�]�@f.�H�=� H��t    H�5�    UH)�H��H��H��H��?H�H��tH��      H��t
                                                                     ]��f�]�@f.��=y      u/H�=W  UH��t
����H����Q       ]����fDUH��]�f���UH��H���}�H�u��}�uH�=������KH�E�H�H�H�5�H��������uH�=��i����H�E�H�H�H��H�=:��X������DAWAVI��AUATL�%F UH�-F SA��I��L)�H�H�������H��t 1��L��L��D��A��H��H9�u�H�[]A\A]A^A_Ðf.���H�H��Hello user! Pass me a -h to learn what I can do!-hOh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_755f3544}I don't know what '%s' means! I do know what -h means though!
```

`-a`選項代表將二進位檔案視為ASCII檔案

或者用`objdump`指令

```console
$ objdump -M intel -s -j .rodata warm

warm:     file format elf64-x86-64

Contents of section .rodata:
 07d0 01000200 00000000 48656c6c 6f207573  ........Hello us
 07e0 65722120 50617373 206d6520 61202d68  er! Pass me a -h
 07f0 20746f20 6c656172 6e207768 61742049   to learn what I
 0800 2063616e 20646f21 002d6800 00000000   can do!.-h.....
 0810 4f682c20 68656c70 3f204920 61637475  Oh, help? I actu
 0820 616c6c79 20646f6e 27742064 6f206d75  ally don't do mu
 0830 63682c20 62757420 4920646f 20686176  ch, but I do hav
 0840 65207468 69732066 6c616720 68657265  e this flag here
 0850 3a207069 636f4354 467b6231 73637531  : picoCTF{b1scu1
 0860 74735f34 6e645f67 72347679 5f373535  ts_4nd_gr4vy_755
 0870 66333534 347d0000 4920646f 6e277420  f3544}..I don't 
 0880 6b6e6f77 20776861 74202725 7327206d  know what '%s' m
 0890 65616e73 21204920 646f206b 6e6f7720  eans! I do know 
 08a0 77686174 202d6820 6d65616e 73207468  what -h means th
 08b0 6f756768 210a00                      ough!..         
 ```

或者更硬核點，用`gdb`找到字串的位置再印出來

```console
$ gdb -q warm
GEF for linux ready, type `gef' to start, `gef config' to configure
90 commands loaded and 5 functions added for GDB 12.1 in 1.78ms using Python engine 3.10
Reading symbols from warm...
gef➤  disas main 
Dump of assembler code for function main:
   0x00000000000006da <+0>:     push   rbp
   0x00000000000006db <+1>:     mov    rbp,rsp
   0x00000000000006de <+4>:     sub    rsp,0x10
   0x00000000000006e2 <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x00000000000006e5 <+11>:    mov    QWORD PTR [rbp-0x10],rsi
   0x00000000000006e9 <+15>:    cmp    DWORD PTR [rbp-0x4],0x1
   0x00000000000006ed <+19>:    jne    0x6fd <main+35>
   0x00000000000006ef <+21>:    lea    rdi,[rip+0xe2]        # 0x7d8
   0x00000000000006f6 <+28>:    call   0x590 <puts@plt>
   0x00000000000006fb <+33>:    jmp    0x748 <main+110>
   0x00000000000006fd <+35>:    mov    rax,QWORD PTR [rbp-0x10]
   0x0000000000000701 <+39>:    add    rax,0x8
   0x0000000000000705 <+43>:    mov    rax,QWORD PTR [rax]
   0x0000000000000708 <+46>:    lea    rsi,[rip+0xfa]        # 0x809
   0x000000000000070f <+53>:    mov    rdi,rax
   0x0000000000000712 <+56>:    call   0x5b0 <strcmp@plt>
   0x0000000000000717 <+61>:    test   eax,eax
   0x0000000000000719 <+63>:    jne    0x729 <main+79>
   0x000000000000071b <+65>:    lea    rdi,[rip+0xee]        # 0x810
   0x0000000000000722 <+72>:    call   0x590 <puts@plt>
   0x0000000000000727 <+77>:    jmp    0x748 <main+110>
   0x0000000000000729 <+79>:    mov    rax,QWORD PTR [rbp-0x10]
   0x000000000000072d <+83>:    add    rax,0x8
   0x0000000000000731 <+87>:    mov    rax,QWORD PTR [rax]
   0x0000000000000734 <+90>:    mov    rsi,rax
   0x0000000000000737 <+93>:    lea    rdi,[rip+0x13a]        # 0x878
   0x000000000000073e <+100>:   mov    eax,0x0
   0x0000000000000743 <+105>:   call   0x5a0 <printf@plt>
   0x0000000000000748 <+110>:   nop
   0x0000000000000749 <+111>:   leave  
   0x000000000000074a <+112>:   ret    
End of assembler dump.
```

可以看出目標字串在`0x810`，用`x/s`指令印出來

```gdb
gef➤  x/s 0x810
0x810:  "Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_755f3544}"
```
