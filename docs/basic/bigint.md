---
tags:
  - 基础算法
  - 高精度
---

众所周知，C++ 的 `int` 类型只能表示 $[-2^{31}, 2^{31}-1]$ 的整数，而 `long long` 类型只能表示 $[-2^{63}, 2^{63}-1]$ 的整数。如果要表示更大的整数，就需要用到高精度整数。

我们可以以 `int` 数组的形式来表示高精度整数，数组中的每个元素表示一位。

例如，我们可以用一个长度为 5 的数组来表示一个 5 位的整数，数组中的每个元素表示这个整数的一位，例如 `a[0]` 表示这个整数的个位，`a[1]` 表示这个整数的十位，以此类推。

部分语言如 Python、Java 自带高精度，可以直接使用。

定义：

```cpp
struct BigInteger {
    int a[1000];
    BigInteger() { memset(a, 0, sizeof(a)); }
    BigInteger operator + (const int x) {
        BigInteger c;
        c.a[0] = a[0] + x;
        for (int i = 1; i < 1000; i++) {
            c.a[i] = a[i];
            if (c.a[i - 1] >= 10) {
                c.a[i - 1] -= 10;
                c.a[i]++;
            }
        }
        return c;
    }
    BigInteger operator + (const BigInteger b) {
        BigInteger c;
        for (int i = 0; i < 1000; i++) {
            c.a[i] = a[i] + b.a[i];
            if (c.a[i - 1] >= 10) {
                c.a[i - 1] -= 10;
                c.a[i]++;
            }
        }
        return c;
    }
    BigInteger operator - (const int x) {
        BigInteger c;
        c.a[0] = a[0] - x;
        for (int i = 1; i < 1000; i++) {
            c.a[i] = a[i];
            if (c.a[i - 1] < 0) {
                c.a[i - 1] += 10;
                c.a[i]--;
            }
        }
        return c;
    }
    BigInteger operator - (const BigInteger b) {
        BigInteger c;
        for (int i = 0; i < 1000; i++) {
            c.a[i] = a[i] - b.a[i];
            if (c.a[i - 1] < 0) {
                c.a[i - 1] += 10;
                c.a[i]--;
            }
        }
        return c;
    }
    BigInteger operator * (const int x) {
        BigInteger c;
        for (int i = 0; i < 1000; i++) {
            c.a[i] = a[i] * x;
            if (c.a[i - 1] >= 10) {
                c.a[i - 1] -= 10;
                c.a[i]++;
            }
        }
        return c;
    }
    BigInteger operator * (const BigInteger b) {
        BigInteger c;
        for (int i = 0; i < 1000; i++) {
            for (int j = 0; j < 1000; j++) {
                c.a[i + j] += a[i] * b.a[j];
                if (c.a[i + j - 1] >= 10) {
                    c.a[i + j - 1] -= 10;
                    c.a[i + j]++;
                }
            }
        }
        return c;
    }
    BigInteger operator / (const int x) {
        BigInteger c;
        for (int i = 999; i >= 0; i--) {
            c.a[i] = a[i] / x;
            a[i - 1] += (a[i] % x) * 10;
        }
        return c;
    }
    BigInteger operator / (const BigInteger b) {
        BigInteger c;
        for (int i = 999; i >= 0; i--) {
            c.a[i] = a[i] / b.a[i];
            a[i - 1] += (a[i] % b.a[i]) * 10;
        }
        return c;
    }
    BigInteger operator % (const int x) {
        BigInteger c;
        for (int i = 999; i >= 0; i--) {
            c.a[i] = a[i] % x;
            a[i - 1] += (a[i] % x) * 10;
        }
        return c;
    }
    BigInteger operator % (const BigInteger b) {
        BigInteger c;
        for (int i = 999; i >= 0; i--) {
            c.a[i] = a[i] % b.a[i];
            a[i - 1] += (a[i] % b.a[i]) * 10;
        }
        return c;
    }
    bool operator < (const BigInteger b) {
        for (int i = 999; i >= 0; i--) {
            if (a[i] != b.a[i]) return a[i] < b.a[i];
        }
        return false;
    }
    bool operator > (const BigInteger b) {
        for (int i = 999; i >= 0; i--) {
            if (a[i] != b.a[i]) return a[i] > b.a[i];
        }
        return false;
    }
    bool operator <= (const BigInteger b) {
        for (int i = 999; i >= 0; i--) {
            if (a[i] != b.a[i]) return a[i] <= b.a[i];
        }
        return true;
    }
    bool operator >= (const BigInteger b) {
        for (int i = 999; i >= 0; i--) {
            if (a[i] != b.a[i]) return a[i] >= b.a[i];
        }
        return true;
    }
    bool operator == (const BigInteger b) {
        for (int i = 999; i >= 0; i--) {
            if (a[i] != b.a[i]) return false;
        }
        return true;
    }
    bool operator != (const BigInteger b) {
        for (int i = 999; i >= 0; i--) {
            if (a[i] != b.a[i]) return true;
        }
        return false;
    }
};
```

在使用FFT的情况下，可以将复杂度从 $O(n^2)$ 下降到 $O(n \log n)$。