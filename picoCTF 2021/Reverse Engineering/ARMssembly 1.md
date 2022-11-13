# ARMssembly 1

[題目連結](https://play.picoctf.org/practice/challenge/111)

一樣懶得看組語:Pepega:，用`aarch64-linux-gnu-as`變成執行檔丟到IDA pro反編譯，
沒想到IDA聰明到可以化簡func裡面在做的事：

```c
#include <stdio.h>
#include <stdlib.h>

int func(int a1)
{
    // (85 << 6) // 3 = 0x715
    return 1813 - a1;
}

int main(int argc, char **argv)
{
    int a1 = atoi(argv[1]);
    if (func(a1))
        puts("You Lose :(");
    else
        puts("You win!");
}
```

所以flag就是`picoCTF{00000715}`。
