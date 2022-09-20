# Wireshark doo dooo do doo...

[題目連結](https://play.picoctf.org/practice/challenge/115)

先下載檔案

```console
$ wget -q https://mercury.picoctf.net/static/ea41c400c3c7b4a63406e5e607d362ab/shark1.pcapng
$ file shark1.pcapng
shark1.pcapng: pcapng capture file - version 1.0
```

這是wireshark的封包截取檔案，用wireshark開啟，有一堆連到某個api server的TCP封包，
發現response都被加密過，也看不出有什麼特別的地方。  
用follow TCP stream (Analyze > Follow > TCP Stream)，可以找到在stream 5有一個特別的封包，
它的body包含一個看起來像flag形狀的東西。

![screenshot](./Wireshark%20doo%20dooo%20do%20doo....webp)

有點像rot13，用[Mod 26](../Cryptography/Mod%2026.md)裡的`rot13.py`試試，結果就能得到flag

```bash
$ echo 'Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}' | py rot13.py        
The flag is picoCTF{p33kab00_1_s33_u_deadbeef}
```
