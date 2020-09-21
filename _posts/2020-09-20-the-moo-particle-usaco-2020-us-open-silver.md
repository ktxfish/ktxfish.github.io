---
title: The Moo Particle | USACO 2020 US Open Silver
date: 2020-09-20 22:15:00
category: USACO
tags:
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("moop.in");
std::ofstream fout("moop.out");

int N;
struct Par { int x, y; } par[MAX_N];
int minl[MAX_N], maxr[MAX_N];
int ans = 1;

int main()
{
    fin >> N;
    for (int i = 0; i < N; ++i)
        fin >> par[i].x >> par[i].y;
    std::sort(par, par + N, [](const Par &a, const Par &b)
        { return (a.x == b.x ? a.y < b.y : a.x < b.x); });
    minl[0] = par[0].y;
    for (int i = 1; i < N; ++i)
        minl[i] = std::min(minl[i - 1], par[i].y);
    maxr[N - 1] = par[N - 1].y;
    for (int i = N - 2; i >= 0; --i)
        maxr[i] = std::max(maxr[i + 1], par[i].y);
    for (int i = 1; i < N; ++i)
        if (minl[i - 1] > maxr[i])
            ++ans;
    fout << ans << '\n';
    return 0;
}
```
