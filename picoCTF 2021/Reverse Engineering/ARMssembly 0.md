# ARMssembly 0

[題目連結](https://play.picoctf.org/practice/challenge/160)

這題的檔案是arm的組語，我懶得看組語:Pepega:，所以用`aarch64-linux-gnu-gcc`
變成執行檔丟到IDA pro反編譯，整個程式其實很短：

```c
#include <stdio.h>
#include <stdlib.h>

int func1(int a1, int a2)
{
    if (a1 <= a2)
        return a2;
    return a1;
}

int main(int argc, const char **argv)
{
    int v1 = atoi(argv[1]);
    int v2 = atoi(argv[2]);
    int v3 = func1(v1, v2);
    printf("Result: %ld\n", v3);
    return 0;
}
```

所以flag就是1765227561和1830628817中較大的那個數字。

```python
# picoCTF{6d1d2dd1}
print(f"picoCTF{{{1830628817:x}}}")
```
