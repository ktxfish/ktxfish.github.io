---
title: Meetings | USACO 2019 Dec Silver
date: 2020-08-29 15:20:00
category: USACO
tags:
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>
#include <queue>

#define MAX_N 50000

std::ifstream fin("meetings.in");
std::ofstream fout("meetings.out");

struct Cow
{
    int w, x, d;
};

int N, L, W, T;
Cow cow[MAX_N];
int weight[MAX_N];
std::queue<int> left;
int meet;

int main()
{
    fin >> N >> L;
    for (int n = 0; n < N; ++n)
        fin >> cow[n].w >> cow[n].x >> cow[n].d, W += cow[n].w;

    std::sort(cow, cow + N, [](const Cow &a, const Cow &b){ return a.x < b.x; });
    for (int n = 0; n < N; ++n)
        weight[n] = cow[n].w;
    std::sort(cow, cow + N, [](const Cow &a, const Cow &b)
        { return (a.d == -1 ? a.x : L - a.x) < (b.d == -1 ? b.x : L - b.x); });
    for (int n = 0, l = 0, r = N - 1; W > 0; ++n)
        if (cow[n].d == -1)
            W -= 2 * weight[l++], T = cow[n].x;
        else
            W -= 2 * weight[r--], T = L - cow[n].x;

    std::sort(cow, cow + N, [](const Cow &a, const Cow &b){ return a.x < b.x; });
    for (int n = 0; n < N; ++n)
        if (cow[n].d == -1)
        {
            while (!left.empty() && cow[n].x - left.front() > 2 * T)
                left.pop();
            meet += left.size();
        }
        else
            left.push(cow[n].x);
    fout << meet << '\n';
    return 0;
}
```
