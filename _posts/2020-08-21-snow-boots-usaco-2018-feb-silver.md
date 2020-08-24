---
title: Snow Boots | USACO 2018 Feb Silver
date: 2020-08-21 14:45:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 250

std::ifstream fin("snowboots.in");
std::ofstream fout("snowboots.out");

int N, B;
int tile[MAX_N + 1], boot[MAX_N + 1];

void search(int n, int s, int d, int b)
{
    for (int i = 1; i <= d && n + 1 < N; ++i)
        if (tile[n + i] <= s && boot[n + i] == -1)
            boot[n + i] = b, search(n + i, s, d, b);
}

int main()
{
    fin >> N >> B;
    for (int n = 0; n < N; ++n)
        fin >> tile[n], boot[n] = -1;
    for (int b = 0; b < B; ++b)
    {
        int s, d;
        fin >> s >> d;
        for (int i = 1; i <= d && i < N; ++i)
            if (tile[i] <= s && boot[i] == -1)
                boot[i] = b, search(i, s, d, b);
        for (int n = 1; n < N; ++n)
            if (tile[n] <= s && boot[n] != -1 && boot[n] != b)
                search(n, s, d, b);
    }
    fout << boot[N - 1] << '\n';
    return 0;
}
```
