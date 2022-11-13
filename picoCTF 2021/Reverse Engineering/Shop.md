# Shop

[題目連結](https://play.picoctf.org/practice/challenge/134)

```console
$ file source
source: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, Go BuildID=-vzeqZwk-BVPkjaCfBwI/0p7moXHKZKzCRDvyyGAW/qBY3Sw8xC5RQBA2lE1Ni/wve_VYHzkLpmj9h9LR5b, with debug_info, not stripped
```

連到題目連結後，可以看到一個購物網站，但是錢不夠買flag。用IDA打開來看，
發現是用Go寫的，在`main_main() -> main_openShop() -> main_menu()`
中可以看到整個程式的邏輯，發現它沒有判斷買的數量是負數這件事，所以可以先買-6個10元
的東西，這樣就能湊到100元買flag了。

```python
flag = [112, 105, 99, 111, 67, 84, 70, 123,
        98, 52, 100, 95, 98, 114, 111, 103,
        114, 97, 109, 109, 101, 114, 95, 98,
        97, 54, 98, 56, 99, 100, 102, 125]

# picoCTF{b4d_brogrammer_ba6b8cdf}
print("".join(chr(i) for i in flag))
```
