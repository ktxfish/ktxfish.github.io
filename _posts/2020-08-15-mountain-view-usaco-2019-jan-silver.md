---
title: Mountain View | USACO 2019 Jan Silver
date: 2020-08-15 23:00:00
category: USACO
tags:
- pair
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>
#include <utility>

#define MAX_N 100000

std::ifstream fin("mountains.in");
std::ofstream fout("mountains.out");

int N;
std::pair<int, int> peak[MAX_N]; // y, x
int left_base[MAX_N], right_base[MAX_N];

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> peak[n].second >> peak[n].first;
    std::sort(peak, peak + N);
    for (int n = 0; n < N; ++n)
    {
        left_base[n] = peak[n].second - peak[n].first;
        right_base[n] = peak[n].second + peak[n].first;
    }
    int visible = N;
    for (int n = 0; n < N; ++n)
        for (int m = n + 1; m < N; ++m)
            if (left_base[n] >= left_base[m] && right_base[n] <= right_base[m])
            {
                --visible;
                break;
            }
    fout << visible << '\n';
    return 0;
}
```
