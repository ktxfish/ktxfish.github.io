---
title: Cow Dance Show | USACO 2017 Jan Silver
date: 2020-08-24 16:15:00
category: USACO
tags:
- binary search
---

<!--more-->

```c++
#include <fstream>
#include <functional>
#include <queue>

#define MAX_N 10000

std::ifstream fin("cowdance.in");
std::ofstream fout("cowdance.out");

int N, T;
int duration[MAX_N];

bool in_time(int k)
{
    int t = 0;
    std::priority_queue<int, std::vector<int>, std::greater<int>> leaving;
    for (int n = 0; n < k; ++n)
        leaving.push(duration[n]);
    int w = 0;
    while (!leaving.empty())
    {
        t = leaving.top();
        leaving.pop();
        if (w < N - k)
            leaving.push(t + duration[k + w++]);
    }
    if (t <= T)
        return true;
    return false;
}

int search(int beg, int end)
{
    if (beg == end)
        return beg;
    if (beg + 1 == end)
    {
        if (in_time(beg))
            return beg;
        return end;
    }
    int mid = (beg + end) / 2;
    if (in_time(mid))
        return search(beg, mid);
    return search(mid + 1, end);
}

int main()
{
    fin >> N >> T;
    for (int n = 0; n < N; ++n)
        fin >> duration[n];
    fout << search(1, N) << '\n';
    return 0;
}
```
