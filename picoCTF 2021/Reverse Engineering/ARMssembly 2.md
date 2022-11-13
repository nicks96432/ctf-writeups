# ARMssembly 2

[題目連結](https://play.picoctf.org/practice/challenge/150)

反正我就是不想看組語:Pepega:，一樣用`aarch64-linux-gnu-as`變成執行檔丟到IDA pro反編譯，
出來的結果長這樣：

```c
#include <stdio.h>
#include <stdlib.h>

unsigned int func1(int a1)
{
    unsigned int v2 = 0;
    for (int i = 0; i < a1; ++i)
        v2 += 3;
    return v2;
}

int main(int argc, char **argv)
{
    int v3 = atoi(argv[1]);
    unsigned int v4 = func1(v3);
    return printf("Result: %ld\n", v4);
}
```

算出來是`(3848786505 * 3) & 0xffffffff = 0xb03776db`。
所以flag是`picoCTF{b03776db}`。
