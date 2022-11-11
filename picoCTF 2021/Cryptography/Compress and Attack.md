# Compress and Attack

[題目連結](https://play.picoctf.org/practice/challenge/127)

這題用的加密方法是Salsa20，目前沒有任何公開的破解方式，而且加密金鑰還會一直換。實在想不到
要怎麼解。所以我稍微偷看一下解答:Pepega:，然後找到這個
[SO上的回答](https://stackoverflow.com/a/30644897/13248907)。所以看起來是要針對
zlib去破解。

zlib用的是huffman coding，然後因為題目會把input加上flag一起壓縮，所以只要input和flag
不一樣，壓縮出來的東西就會更長，而且Salsa20是stream cipher，他的密文和明文一樣長。
所以我們可以用zlib的這個特性來猜flag。根據題目，flag只包含字母、底線和大括號，所以我們
一次試一個字元，如果他不會讓壓縮後的長度變長，那他就是flag的一部分。這樣就可以解出flag。

```python
import string

from pwn import context, remote

context.log_level = "error"

flag = "picoCTF{"
init_length = 48
alphabet = "_}" + string.ascii_letters

while True:
    r = remote("mercury.picoctf.net", 29675)
    for c in alphabet:
        try:
            r.sendline((flag + c).encode("utf-8"))
            r.recvlines(2)
            length = int(r.recvline(keepends=False).decode("utf-8"))
            if length == init_length:
                flag += c
                print(flag, end="\r")
                break
        except EOFError:
            r = remote("mercury.picoctf.net", 29675)
    if flag[-1] == "}":
        # picoCTF{sheriff_you_solved_the_crime}
        print(flag)
        break
```

補充: 這題的考點是[CRIME](https://en.wikipedia.org/wiki/CRIME)。
