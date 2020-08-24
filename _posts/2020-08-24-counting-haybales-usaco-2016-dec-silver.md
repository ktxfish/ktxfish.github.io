---
title: Counting Haybales | USACO 2016 Dec Silver
date: 2020-08-24 11:00:00
category: USACO
tags:
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("haybales.in");
std::ofstream fout("haybales.out");

int N, Q;
int pos[MAX_N];

int main()
{
    fin >> N >> Q;
    for (int n = 0; n < N; ++n)
        fin >> pos[n];
    std::sort(pos, pos + N);
    for (int q = 0; q < Q; ++q)
    {
        int a, b;
        fin >> a >> b;
        fout << std::upper_bound(pos, pos + N, b) - std::lower_bound(pos, pos + N, a)
             << '\n';
    }
    return 0;
}
```
