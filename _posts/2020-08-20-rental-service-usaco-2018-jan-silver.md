---
title: Rental Service | USACO 2018 Jan Silver
date: 2020-08-20 18:00:00
category: USACO
tags:
- prefix sum
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000
#define MAX_M 100000
#define MAX_R 100000

std::ifstream fin("rental.in");
std::ofstream fout("rental.out");

struct Shop
{
    int q, p;
};

int N, M, R;
int prod[MAX_N + 1];
Shop shop[MAX_M + 1];
long long sell[MAX_N + 1], rent[MAX_R + 1];
long long max_prof;

int main()
{
    fin >> N >> M >> R;
    for (int n = 0; n < N; ++n)
        fin >> prod[n];
    for (int m = 0; m < M; ++m)
        fin >> shop[m].q >> shop[m].p;
    for (int r = 0; r < R; ++r)
        fin >> rent[r];
    std::sort(prod, prod + N, [](const int &a, const int &b) { return a > b; });
    std::sort(shop, shop + M, [](const Shop &a, const Shop &b) { return a.p > b.p; });
    std::sort(rent, rent + R, [](const long long &a, const long long &b) { return a > b; });
    for (int n = 0, m = 0; n < N; ++n)
    {
        if (n)
            sell[n] = sell[n - 1];
        while (prod[n])
        {
            if (m == M)
                break;
            int x = std::min(prod[n], shop[m].q);
            sell[n] += x * shop[m].p;
            prod[n] -= x;
            shop[m].q -= x;
            if (!shop[m].q)
                ++m;
        }
    }
    for (int r = 1; r < R; ++r)
        rent[r] += rent[r - 1];
    max_prof = sell[N - 1];
    for (int r = 0; r < R && r < N; ++r)
        max_prof = std::max(max_prof, sell[N - r - 2] + rent[r]);
    fout << max_prof << '\n';
    return 0;
}
```
