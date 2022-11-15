# ARMSeembly 4

[題目連結](https://play.picoctf.org/practice/challenge/183)

好耶，又可以直接用`aarch64-linux-gnu-gcc`編譯完丟給IDA pro反編譯了。
編譯完之後大概長這樣：

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char **argv)
{
    int v3 = atoi(argv[1]);

    if (v3 <= 100)
        v3 = 7;
    else
    {
        v3 += 100;
        if (v3 > 499)
            v3 += 15;
        else
            v3 -= 86;
    }

    return printf("Result: %ld\n", v3);
}
```

所以把參數`3964545182`帶進去，就知道flag是`picoCTF{ec4e2911}`了。
