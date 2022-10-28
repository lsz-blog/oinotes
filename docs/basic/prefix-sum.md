---
tags:
  - 基础算法
  - 前缀和
---

设我们有一个一维数组 $\{a_n\}$。

考虑一个问题：给定一个区间 $[l, r]$，求 $\sum_{i=l}^r a_i$。

我们可以先使用暴力的方法，对于每个询问，遍历 $[l, r]$，时间复杂度 $O(nq)$：

```cpp
int query (int l, int r) {
    int sum = 0;
    for (int i = l; i <= r; i++)
        sum += a[i];
    return sum;
}
```

我们可以发现，$\sum_{i=l}^r a_i = \sum_{i=1}^{r}a_i-\sum_{i=1}^{l-1}a_i$，所以我们可以在 $O(n)$ 时间内预计算 $\{s_n\}=\sum_{i=1}^{n}a_i$：

```cpp
for (int i = 1; i <= n; i++)
    s[i] = s[i-1] + a[i];
```

这样，对于每个询问，我们只需要 $O(1)$ 的时间复杂度：

```cpp
printf("%d\n", s[r] - s[l-1]);
```