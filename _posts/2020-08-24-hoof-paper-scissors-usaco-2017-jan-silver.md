---
title: Hoof, Paper, Scissors | USACO 2017 Jan Silver
date: 2020-08-24 17:20:00
category: USACO
tags:
- prefix sum
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("hps.in");
std::ofstream fout("hps.out");

int N;
int ch[MAX_N], cp[MAX_N], cs[MAX_N];
int lh[MAX_N], lp[MAX_N], ls[MAX_N];
int rh[MAX_N], rp[MAX_N], rs[MAX_N];
int max_score;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        char c;
        fin >> c;
        if (c == 'H')
            ++ch[n], ++lp[n], ++rp[n];
        else if (c == 'P')
            ++cp[n], ++ls[n], ++rs[n];
        else
            ++cs[n], ++lh[n], ++rh[n];
    }
    for (int n = 1; n < N; ++n)
        ch[n] += ch[n - 1], cp[n] += cp[n - 1], cs[n] += cs[n - 1],
        lh[n] += lh[n - 1], lp[n] += lp[n - 1], ls[n] += ls[n - 1];
    for (int n = N - 2; n >= 0; --n)
        rh[n] += rh[n + 1], rp[n] += rp[n + 1], rs[n] += rs[n + 1];
    for (int n = 0; n < N; ++n)
        max_score = std::max({max_score, lh[n] + rp[n], lh[n] + rs[n],
                                         lp[n] + rh[n], lp[n] + rs[n],
                                         ls[n] + rh[n], ls[n] + rp[n]});
    fout << max_score << '\n';
    return 0;
}
```
