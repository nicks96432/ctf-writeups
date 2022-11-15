# ARMssembly 3

[題目連結](https://play.picoctf.org/practice/challenge/106)

一樣編譯好丟IDA pro，出來的結果長這樣：

```c
#include <stdio.h>
#include <stdlib.h>

int func2(int a1)
{
    return a1 + 3;
}

int func1(int a1)
{
    int v3 = 0;
    while (a1)
    {
        if ((a1 & 1) != 0)
            v3 = func2(v3);
        a1 >>= 1;
    }

    return v3;
}

int main(int argc, const char **argv)
{
    int v3 = atoi(argv[1]);
    int v4 = func1(v3);
    return printf("Result: %ld\n", v4);
}
```

可以看成印出`__builtin_popcount(atoi(argv[1]))`，所以
`3350728462`會得到`0x30`，所以flag就是`picoCTF{00000030}`。
