# Hurry up! Wait!

[題目連結](https://play.picoctf.org/practice/challenge/165)

```console
$ file svchost.exe 
svchost.exe: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=814d4541585e38937399b50bf05853bc665f0629, stripped
```

雖然叫做 `svchost.exe`，但是這是一個 ELF 檔案，直接執行需要`libgnat-7.so`，這個版本實在太舊，懶得裝了。
用IDA打開來看和一點估狗，推測這應該是用一種叫做ada的語言寫的。但是在`sub_298A()`可以看到他會等
1000000000000000秒之後把flag逐個字元印出來。所以flag是`picoCTF{d15a5m_ftw_dfbdc5d}`。
