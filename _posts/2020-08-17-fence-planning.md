---
title: Fence Planning | USACO 2019 US Open Silver
date: 2020-08-17 16:25:00
category: USACO
tags:
- flood fill
---

<!--more-->

```c++
#include <fstream>
#include <vector>

#define MAX_N 100000
#define MAX_X 100000000
#define MAX_Y 100000000

std::ifstream fin("fenceplan.in");
std::ofstream fout("fenceplan.out");

struct Cow
{
    int x, y;
    int net = 0;
    std::vector<int> moo;
};

int N, M;
Cow cow[MAX_N];
int x1, x2, y1, y2;

void flood_fill(int n, int k)
{
    cow[n].net = k;
    if (cow[n].x < x1)
        x1 = cow[n].x;
    if (cow[n].x > x2)
        x2 = cow[n].x;
    if (cow[n].y < y1)
        y1 = cow[n].y;
    if (cow[n].y > y2)
        y2 = cow[n].y;
    for (int m : cow[n].moo)
        if (!cow[m].net)
            flood_fill(m, k);
}

int main()
{
    fin >> N >> M;
    for (int n = 0; n < N; ++n)
        fin >> cow[n].x >> cow[n].y;
    for (int m = 0; m < M; ++m)
    {
        int a, b;
        fin >> a >> b;
        cow[a - 1].moo.push_back(b - 1);
        cow[b - 1].moo.push_back(a - 1);
    }
    int min_perim = 2 * (MAX_X + MAX_Y);
    for (int n = 0, k = 0; n < N; ++n)
        if (!cow[n].net)
        {
            x1 = MAX_X, x2 = 0, y1 = MAX_Y, y2 = 0;
            flood_fill(n, ++k);
            int perim;
            if (x1 == x2)
                perim = y2 - y1;
            else if (y1 == y2)
                perim = x2 - x1;
            else
                perim = 2 * (x2 - x1 + y2 - y1);
            if (perim < min_perim)
                min_perim = perim;
        }
    fout << min_perim << '\n';
    return 0;
}
```
