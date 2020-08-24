---
title: Convention | USACO 2018 Dec Silver
date: 2020-08-14 13:00:00
category: USACO
tags:
- binary search
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000
#define MAX_M 100000
#define MAX_T 1000000000

std::ifstream fin("convention.in");
std::ofstream fout("convention.out");

int N, M, C;
int times[MAX_N];

bool fits(int wait)
{
    int m = 1, size = 1, first = times[0];
    for (int n = 1; n < N; ++n)
        if (size < C && times[n] - first <= wait)
            ++size;
        else
        {
            first = times[n];
            size = 1;
            ++m;
        }
    if (m <= M)
        return true;
    return false;
}

int search(int beg, int end)
{
    if (beg == end)
        return beg;
    if (beg + 1 == end)
    {
        if (fits(beg))
            return beg;
        return end;
    }
    int mid = (beg + end) / 2;
    if (fits(mid))
        return search(beg, mid);
    return search(mid + 1, end);
}

int main()
{
    fin >> N >> M >> C;
    for (int n = 0; n < N; ++n)
        fin >> times[n];
    std::sort(times, times + N);
    fout << search(0, MAX_T) << '\n';
    return 0;
}
```
