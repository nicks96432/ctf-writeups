# Play Nice

[題目連結](https://play.picoctf.org/practice/challenge/114)

照著題目給的`playfair.py`反過來做就行了

```python
import sys

from pwn import context, remote

context.log_level = "error"
SQUARE_SIZE = 6


def generate_square(alphabet):
    assert len(alphabet) == pow(SQUARE_SIZE, 2)
    matrix = []
    for i, letter in enumerate(alphabet):
        if i % SQUARE_SIZE == 0:
            row = []
        row.append(letter)
        if i % SQUARE_SIZE == (SQUARE_SIZE - 1):
            matrix.append(row)
    return matrix


def get_index(letter, matrix):
    for row in range(SQUARE_SIZE):
        for col in range(SQUARE_SIZE):
            if matrix[row][col] == letter:
                return (row, col)
    print("letter not found in matrix.")
    sys.exit()


def decrypt_pair(pair, matrix):
    p1 = get_index(pair[0], matrix)
    p2 = get_index(pair[1], matrix)

    if p1[0] == p2[0]:
        return (
            matrix[p1[0]][(p1[1] - 1) % SQUARE_SIZE]
            + matrix[p2[0]][(p2[1] - 1) % SQUARE_SIZE]
        )
    if p1[1] == p2[1]:
        return (
            matrix[(p1[0] - 1) % SQUARE_SIZE][p1[1]]
            + matrix[(p2[0] - 1) % SQUARE_SIZE][p2[1]]
        )

    return matrix[p1[0]][p2[1]] + matrix[p2[0]][p1[1]]


def decrypt_string(s, matrix):
    result = ""
    plain = s if len(s) % 2 == 0 else s + "0"
    for i in range(0, len(plain), 2):
        result += decrypt_pair(plain[i : i + 2], matrix)
    return result


with remote("mercury.picoctf.net", 30568) as r:
    key = r.recvline().decode("utf-8").split()[-1]
    cipher = r.recvline().decode("utf-8").split()[-1]

    m = generate_square(key)
    r.sendline(decrypt_string(cipher, m).encode("utf-8"))
    # 007d0a696aaad7fb5ec21c7698e4f754
    print(r.recvline().decode("utf-8").split()[-1])
```
