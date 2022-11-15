# Easy as GDB

[題目連結](https://play.picoctf.org/practice/challenge/122)

```console
$ file brute
brute: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=fee4a6835b0f9de77915192abee17e385706dfd3, stripped
```

打開IDA pro反編譯之後可以看到是個普通的flag checker，輸入字串之後它會把字串做很酷的xor
加密，然後再換個順序，再把加密過的flag也換一次順序，最後比較兩者是否相同。所以反著對加密過
的flag做一樣的事情就可以得到flag了。

```c
#include <stdio.h>

char encrypted_flag[30] = {
    0x7A, 0x2E, 0x6E, 0x68, 0x1D, 0x65, 0x16, 0x7C,
    0x6D, 0x43, 0x6F, 0x36, 0x65, 0x62, 0x40, 0x16,
    0x43, 0x62, 0x40, 0x3F, 0x58, 0x01, 0x58, 0x33,
    0x62, 0x6B, 0x53, 0x30, 0x38, 0x17
};

int main(void)
{
    for (int i = 29; i > 0; --i)
        for (int j = 0; j < 30 - i + 1; j += i)
        {
            char tmp                  = encrypted_flag[j];
            encrypted_flag[j]         = encrypted_flag[i + j - 1];
            encrypted_flag[i + j - 1] = tmp;
        }

    for (unsigned int passwd = 0xABCF00D; passwd < 0xDEADBEEF; passwd += 0x1FAB4D)
    {
        char passwd_1[4];
        passwd_1[0] = passwd >> 24;
        passwd_1[1] = (passwd >> 16) & 0xFF;
        passwd_1[2] = (passwd >> 8) & 0xFF;
        passwd_1[3] = passwd & 0xFF;
        for (int i = 0; i < 30; ++i)
            encrypted_flag[i] ^= passwd_1[i & 3];
    }

    // picoCTF{I_5D3_A11DA7_e5458cbf}
    for (int i = 0; i < 30; ++i)
        putchar(encrypted_flag[i]);

    return 0;
}
```
