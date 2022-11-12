# Scrambled: RSA

[題目連結](https://play.picoctf.org/practice/challenge/107)

雖然名字叫做RSA，但是這題的flag卻長到爆炸，更奇怪的是輸入的內容長度卻和加密結果長度正相關。
而且輸入`a`得到的密文出現在`ab`的密文裡，不過出現位置每次都有可能不同，但`b`的密文卻沒有出現
在`ab`的密文中。同理，`ab`中`b`的密文(和`b`的密文不同)會出現在`abc`的密文中。

利用這點，`p`的密文一定會出現在flag的密文裡，`pi`中`i`的密文也會出現在flag的密文裡。
所以可以逐個字元把flag全部解出來。

```python
import string

from pwn import context, remote

context.log_level = "error"
ALPHABET = "_{}" + string.digits + string.ascii_letters
r = remote("mercury.picoctf.net", 47987)

encrypted_flag = r.recvline().decode("utf-8").split()[-1]
r.recvlines(2) # n, e

enc_dict = []
flag = "picoCTF{"

tmp = ""
for c in flag:
    r.sendline((tmp + c).encode("utf-8"))
    s = r.recvline(keepends=False).decode("utf-8").split()[-1]
    for enc in enc_dict:
        s = s.replace(enc, "")
    enc_dict.append(s)
    tmp += c

while flag[-1] != "}":
    for c in ALPHABET:
        r.sendline((flag + c).encode("utf-8"))
        s = r.recvline(keepends=False).decode("utf-8").split()[-1]
        for enc in enc_dict:
            s = s.replace(enc, "")
        if s in encrypted_flag:
            enc_dict.append(s)
            flag += c
            break

# picoCTF{bad_1d3a5_7571572}
print(flag)
```
