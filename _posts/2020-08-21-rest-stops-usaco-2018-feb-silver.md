---
title: Rest Stops | USACO 2018 Feb Silver
date: 2020-08-21 10:40:00
category: USACO
tags:
- greedy
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100000

std::ifstream fin("reststops.in");
std::ofstream fout("reststops.out");

int L, N, rf, rb;
int pos[MAX_N], yum[MAX_N];
bool max[MAX_N];
long ttl;

int main()
{
    fin >> L >> N >> rf >> rb;
    for (int n = 0; n < N; ++n)
        fin >> pos[n] >> yum[n];
    int max_y = yum[N - 1];
    max[N - 1] = true;
    for (int n = N - 2; n >= 0; --n)
        if (yum[n] > max_y)
            max_y = yum[n], max[n] = true;
    for (long n = 0, f = 0, b = 0, x = 0; n < N; ++n)
        if (max[n])
        {
            f += (pos[n] - x) * rf;
            b += (pos[n] - x) * rb;
            ttl += (f - b) * yum[n];
            b = f;
            x = pos[n];
        }
    fout << ttl << '\n';
    return 0;
}
```
