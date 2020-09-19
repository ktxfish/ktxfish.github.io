---
title: Triangles | USACO 2020 Feb Silver
date: 2020-09-18 22:30:00
category: USACO
tags:
- prefix sum
---

<!--more-->

```c++
#include <fstream>
#include <algorithm>

#define MAX_N 100000
#define MOD 1000000007

std::ifstream fin("triangles.in");
std::ofstream fout("triangles.out");

int N;
struct Point { int x, y; long long v = 0, h = 0; } pt[MAX_N];
long long ans;

int main()
{
    fin >> N;
    for (int i = 0; i < N; ++i)
        fin >> pt[i].x >> pt[i].y;
    std::sort(pt, pt + N, [](const Point &a, const Point &b){ return (a.x == b.x ? a.y < b.y : a.x < b.x); });
    for (int i = 0; i < N; )
    {
        int n = 1, p = pt[i].y, x = pt[i].x;
        long long r = 0;
        ++i;
        for (; i < N && pt[i].x == x; ++i, ++n)
            r += n * (pt[i].y - p), pt[i].v += r, p = pt[i].y;
    }
    std::sort(pt, pt + N, [](const Point &a, const Point &b){ return (a.x == b.x ? a.y > b.y : a.x < b.x); });
    for (int i = 0; i < N; )
    {
        int n = 1, p = pt[i].y, x = pt[i].x;
        long long r = 0;
        ++i;
        for (; i < N && pt[i].x == x; ++i, ++n)
            r += n * (p - pt[i].y), pt[i].v += r, p = pt[i].y;
    }
    std::sort(pt, pt + N, [](const Point &a, const Point &b){ return (a.y == b.y ? a.x < b.x : a.y < b.y); });
    for (int i = 0; i < N; )
    {
        int n = 1, p = pt[i].x, y = pt[i].y;
        long long r = 0;
        ++i;
        for (; i < N && pt[i].y == y; ++i, ++n)
            r += n * (pt[i].x - p), pt[i].h += r, p = pt[i].x;
    }
    std::sort(pt, pt + N, [](const Point &a, const Point &b){ return (a.y == b.y ? a.x > b.x : a.y < b.y); });
    for (int i = 0; i < N; )
    {
        int n = 1, p = pt[i].x, y = pt[i].y;
        long long r = 0;
        ++i;
        for (; i < N && pt[i].y == y; ++i, ++n)
            r += n * (p - pt[i].x), pt[i].h += r, p = pt[i].x;
    }
    for (int i = 0; i < N; ++i)
        ans += pt[i].v * pt[i].h, ans %= MOD;
    fout << ans << '\n';
    return 0;
}
```
