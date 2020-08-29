---
title: Breed Counting | USACO 2015 Dec Silver
date: 2020-08-29 11:40:00
category: USACO
tags:
- prefix sum
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100000

std::ifstream fin("bcount.in");
std::ofstream fout("bcount.out");

int N, Q;
int sum1[MAX_N + 1], sum2[MAX_N + 1], sum3[MAX_N + 1];

int main()
{
    fin >> N >> Q;
    for (int n = 1; n <= N; ++n)
    {
        sum1[n] = sum1[n - 1];
        sum2[n] = sum2[n - 1];
        sum3[n] = sum3[n - 1];
        int id;
        fin >> id;
        if (id == 1)
            ++sum1[n];
        else if (id == 2)
            ++sum2[n];
        else
            ++sum3[n];
    }
    for (int q = 0; q < Q; ++q)
    {
        int a, b;
        fin >> a >> b;
        fout << sum1[b] - sum1[a - 1] << ' ' << sum2[b] - sum2[a - 1] << ' '
             << sum3[b] - sum3[a - 1] << '\n';
    }
    return 0;
}
```
