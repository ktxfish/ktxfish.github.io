---
title: Social Distancing | USACO 2020 US Open Silver
date: 2020-09-19 16:30:00
category: USACO
tags:
- binary search
- greedy
---

<!--more-->

```c++
#include <fstream>
#include <algorithm>
#include <utility>

#define MAX_M 100000
#define MAX_A 1000000000000000000

std::ifstream fin("socdist.in");
std::ofstream fout("socdist.out");

int N, M;
std::pair<long long, long long> grass[MAX_M];

bool fits(long long d)
{
    int n = N;
    long long l = -MAX_A;
    for (int i = 0; i < M; ++i)
        while (l + d <= grass[i].second)
            --n, l = std::max(grass[i].first, l + d);
    if (n > 0)
        return false;
    return true;
}

long long search(long long beg, long long end)
{
    if (beg == end)
        return end;
    if (beg + 1 == end)
    {
        if (fits(end))
            return end;
        return beg;
    }
    long long mid = (beg + end) / 2;
    if (fits(mid))
        return search(mid, end);
    return search(beg, mid - 1);
}

int main()
{
    fin >> N >> M;
    for (int i = 0; i < M; ++i)
        fin >> grass[i].first >> grass[i].second;
    std::sort(grass, grass + M);
    fout << search(1, MAX_A) << '\n';
    return 0;
}
```
