---
title: Milk Visits | USACO 2019 Dec Silver
date: 2020-08-30 16:20:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>
#include <vector>

#define MAX_N 100000

std::ifstream fin("milkvisits.in");
std::ofstream fout("milkvisits.out");

int N, M, C;
char cow[MAX_N];
std::vector<int> road[MAX_N];
int comp[MAX_N];

void flood_fill(int n)
{
    comp[n] = C;
    for (int r : road[n])
        if (cow[r] == cow[n] && comp[r] == -1)
            flood_fill(r);
}

int main()
{
    fin >> N >> M;
    for (int n = 0; n < N; ++n)
        fin >> cow[n];
    for (int n = 1; n < N; ++n)
    {
        int a, b;
        fin >> a >> b;
        road[a - 1].push_back(b - 1);
        road[b - 1].push_back(a - 1);
    }
    for (int n = 0; n < N; ++n)
        comp[n] = -1;
    for (int n = 0; n < N; ++n)
        if (comp[n] == -1)
            flood_fill(n), ++C;
    for (int m = 0; m < M; ++m)
    {
        int a, b;
        char c;
        fin >> a >> b >> c;
        fout << (comp[a - 1] != comp[b - 1] || cow[a - 1] == c);
    }
    fout << '\n';
    return 0;
}
```
