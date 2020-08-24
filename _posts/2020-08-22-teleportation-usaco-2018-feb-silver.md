---
title: Teleportation | USACO 2018 Feb Silver
category: USACO
date: 2020-08-22 18:00:00
tags:
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>
#include <map>

std::ifstream fin("teleport.in");
std::ofstream fout("teleport.out");

int N;
std::map<int, int> change;
long long min_haul, haul;
int slope, prv_y = -400000000;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        int a, b;
        fin >> a >> b;
        haul += std::abs(a - b);
        if (std::abs(a) < std::abs(a - b))
        {
            change[b] += 2;
            if (a < 0 && a < b || a > 0 && a > b)
                --change[0], --change[2 * b];
            else
                --change[2 * a], --change[2 * b - 2 * a];
        }
    }
    min_haul = haul;
    for (auto c : change)
    {
        haul += (c.first - prv_y) * slope;
        prv_y = c.first;
        slope += c.second;
        if (haul < min_haul)
            min_haul = haul;
    }
    fout << min_haul << '\n';
    return 0;
}
```
