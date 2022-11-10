# New Caesar

[題目連結](https://play.picoctf.org/practice/challenge/158)

這題因為key只有16種的關係，就每個都試一次然後照著原來的code反著做就行了。
輸出的第一個字串長得比較像flag，所以就先試它，沒想到就成功了。

```python
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]


def b16_decode(encoded: str):
    plain = ""
    for i in range(0, len(encoded), 2):
        binary = (ord(encoded[i]) - LOWERCASE_OFFSET) << 4
        binary += ord(encoded[i + 1]) - LOWERCASE_OFFSET
        plain += chr(binary)
    return plain


def shift_back(c: str, k: str):
    t1 = ord(c) - LOWERCASE_OFFSET
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET[(t1 - t2) % len(ALPHABET)]


encoded_flag = (
    "mlnklfnknljflfmhjimkmhjhmljhjomhmmjkjpmmjmjkjpjojgjmjpjojojnjojmmkmlmijimhjmmj"
)

for key in ALPHABET:
    shifted = ""
    for e in encoded_flag:
        shifted += shift_back(e, key)

    flag = b16_decode(shifted)
    for c in flag:
        if c not in string.ascii_letters + string.digits + string.punctuation + " ":
            break
    else:
        print(flag)

# et_tu?_a2da1e18af49f649806988786deb2a6c
# TcNcd.NP!SP T 'PU#(U%#('/%(''&'%STQ!P%R
```
