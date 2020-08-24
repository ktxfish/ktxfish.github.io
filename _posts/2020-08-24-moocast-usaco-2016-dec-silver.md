---
title: Moocast | USACO 2016 Dec Silver
date: 2020-08-24 14:50:00
category: USACO
tags:
- flood fill
---

<!--more-->

```c++
#include <cmath>
#include <fstream>

#define MAX_N 200

std::ifstream fin("moocast.in");
std::ofstream fout("moocast.out");

int N;
int x[MAX_N], y[MAX_N], p[MAX_N];
bool trans[MAX_N][MAX_N];
int max_reach;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> x[n] >> y[n] >> p[n];
    for (int n = 0; n < N; ++n)
        for (int m = 0; m < N; ++m)
            if (std::sqrt(std::pow(x[n] - x[m], 2) + std::pow(y[n] - y[m], 2)) < p[n])
                trans[n][m] = true;
    for (int n = 0; n < N; ++n)
    {
        int reach = 0, r, reachable[MAX_N];
        for (int i = 0; i < N; ++i)
            reachable[i] = (trans[n][i] ? -1 : 0);
        do
        {
            r = 0;
            for (int i = 0; i < N; ++i)
                if (reachable[i] == -1)
                {
                    reachable[i] = 1;
                    ++reach;
                    for (int j = 0; j < N; ++j)
                        if (!reachable[j] && trans[i][j])
                            reachable[j] = -1, ++r;
                }
        }
        while (r);
        if (reach > max_reach)
            max_reach = reach;
    }
    fout << max_reach << '\n';
    return 0;
}
```
