# Let's get dynamic

[題目連結](https://play.picoctf.org/practice/challenge/102)

這次給的是x86的組語，反正遇到組語就先編譯再丟IDA pro。
解出來大概是這樣：

```c
#include <stdint.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    int i;
    char s2[64];
    char s[64];
    char v7[50];
    char v8[50];

    *(uint64_t *)v7      = 0x9ca22a2c6e25f4bbLL;
    *(uint64_t *)&v7[8]  = 0xf48a0773939cacbdLL;
    *(uint64_t *)&v7[16] = 0x0aea4d74d7bf0305LL;
    *(uint64_t *)&v7[24] = 0x1af35b3168a434abLL;
    *(uint64_t *)&v7[32] = 0x2eb55ce116f3170fLL;
    *(uint64_t *)&v7[40] = 0xb398b1ef10a7b466LL;
    *(uint16_t *)&v7[48] = 0x0047;
    *(uint64_t *)v8      = 0xf3f1687811578fd8LL;
    *(uint64_t *)&v8[8]  = 0xb7f42801bfebcfc2LL;
    *(uint64_t *)&v8[16] = 0x519c7a0abb8a6f32LL;
    *(uint64_t *)&v8[24] = 0x64cd254b55f24d91LL;
    *(uint64_t *)&v8[32] = 0x45ec1fb015b15063LL;
    *(uint64_t *)&v8[40] = 0xbb90ede51bfbb868LL;
    *(uint16_t *)&v8[48] = 0x0019;

    fgets(s, 49, stdin);
    for (int i = 0; i < strlen(v7); ++i)
        s2[i] = v8[i] ^ v7[i] ^ i ^ 0x13;
    if (!memcmp(s, s2, 49))
    {
        puts("No, that's not right.");
        return 1;
    }
    else
    {
        puts("Correct! You entered the flag.");
        return 0;
    }
}
```

所以flag可以輕鬆解出來：

```c
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    char v7[50];
    char v8[50];

    *(uint64_t *)v7      = 0x9ca22a2c6e25f4bbLL;
    *(uint64_t *)&v7[8]  = 0xf48a0773939cacbdLL;
    *(uint64_t *)&v7[16] = 0x0aea4d74d7bf0305LL;
    *(uint64_t *)&v7[24] = 0x1af35b3168a434abLL;
    *(uint64_t *)&v7[32] = 0x2eb55ce116f3170fLL;
    *(uint64_t *)&v7[40] = 0xb398b1ef10a7b466LL;
    *(uint16_t *)&v7[48] = 0x0047;
    *(uint64_t *)v8      = 0xf3f1687811578fd8LL;
    *(uint64_t *)&v8[8]  = 0xb7f42801bfebcfc2LL;
    *(uint64_t *)&v8[16] = 0x519c7a0abb8a6f32LL;
    *(uint64_t *)&v8[24] = 0x64cd254b55f24d91LL;
    *(uint64_t *)&v8[32] = 0x45ec1fb015b15063LL;
    *(uint64_t *)&v8[40] = 0xbb90ede51bfbb868LL;
    *(uint16_t *)&v8[48] = 0x0019;

    // picoCTF{dyn4m1c_4n4ly1s_1s_5up3r_us3ful_56e35b54}
    for (int i = 0; i < 49; ++i)
        putchar(v7[i] ^ v8[i] ^ i ^ 0x13);

    return 0;
}
```

其實也不用動態分析，畢竟有IDA :Pepega:。
