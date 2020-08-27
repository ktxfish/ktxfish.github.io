---
title: Paired Up | USACO 2017 US Open Silver
date: 2020-08-26 23:00:00
category: USACO
tags:
- greedy
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("pairup.in");
std::ofstream fout("pairup.out");

struct Rec
{
    int x, y;
};

int N;
Rec rec[MAX_N];
int l, r;
int amt;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> rec[n].x >> rec[n].y;
    std::sort(rec, rec + N, [](const Rec &a, const Rec &b) { return a.y < b.y; });
    r = N - 1;
    while (l < r)
    {
        int m = std::min(rec[l].x, rec[r].x);
        rec[l].x -= m, rec[r].x -= m;
        if (!rec[l].x)
            ++l;
        if (!rec[r].x)
            --r;
        if (rec[l].y + rec[r].y > amt)
            amt = rec[l].y + rec[r].y;
    }
    if (l == r)
        if (rec[l].y * 2 > amt)
            amt = rec[l].y * 2;
    fout << amt << '\n';
    return 0;
}
```
