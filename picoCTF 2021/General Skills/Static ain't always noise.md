# Static ain't always noise

[題目連結](https://play.picoctf.org/practice/challenge/163)

先下載檔案

```bash
wget -q https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/static
wget -q https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/ltdis.sh
```

用題目提供的script反組譯就可以得到flag

```console
$ bash ./ltdis.sh static
Attempting disassembly of ./static ...
Disassembly successful! Available at: ./static.ltdis.x86_64.txt
Ripping strings from binary with file offsets...
Any strings found in ./static have been written to ./static.ltdis.strings.txt with file offset
$ grep picoCTF static.ltdis.strings.txt
   1020 picoCTF{d15a5m_t34s3r_1e6a7731}
```

或者也可以用[Wave-a-flag.md](Wave%20a%20flag.md)中提到的方法
