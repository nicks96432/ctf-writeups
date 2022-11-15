# gogo

[題目連結](https://play.picoctf.org/practice/challenge/171)

```console
$ file enter_password
enter_password: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, Go BuildID=dSGr2TxewgLiHXv2h4V0/X91zd5C2lAkGTLFzXoln/iepyNIwQPue4FGJD5lPl/qJbWXlfMi7bgFI6RKmFg, with debug_info, not stripped
```

又是一個Go編譯出來的ELF檔案，稍微看了一下，是普通的password checker。可以在
`main_checkPassword`看到輸入的字串會和一個key做xor，然後再跟一個固定的字串比較，
所以把那個字串跟key xor就可以得到正確的密碼。

```python
key = b"861836f13e3d627dfa375bdb8389214e"
encrypted_password = [
    0x4a, 0x53, 0x47, 0x5d, 0x41, 0x45, 0x03, 0x54,
    0x5d, 0x02, 0x5a, 0x0a, 0x53, 0x57, 0x45, 0x0d,
    0x05, 0x00, 0x5d, 0x55, 0x54, 0x10, 0x01, 0x0e,
    0x41, 0x55, 0x57, 0x4b, 0x45, 0x50, 0x46, 0x01
]

# reverseengineericanbarelyforward
for c, k in zip(encrypted_password, key):
    print(chr(c ^ k), end="")
```

接下來會問key是什麼東西的hash，
可以在`main_ambush`看到它會把輸入做md5和key做比較。可以上網找md5 database之類的，
或是用john或是hashcat配個rockyou，解出來會是`goldfish`，輸入之後就能得到flag。

```console
$ nc mercury.picoctf.net 35862
Enter Password: reverseengineericanbarelyforward
=========================================
This challenge is interrupted by psociety
What is the unhashed key?
goldfish
Flag is:  picoCTF{p1kap1ka_p1c05729981f}
```
