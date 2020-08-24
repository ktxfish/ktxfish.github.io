---
title: Grass Planting | USACO 2019 Jan Silver
date: 2020-08-15 20:00:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100000

std::ifstream fin("planting.in");
std::ofstream fout("planting.out");

int N;
int nums[MAX_N];
int max;

int main()
{
    fin >> N;
    for (int n = 0; n < N - 1; ++n)
    {
        int a, b;
        fin >> a >> b;
        ++nums[a - 1];
        ++nums[b - 1];
    }
    for (int n = 0; n < N; ++n)
        if (nums[n] > max)
            max = nums[n];
    fout << max + 1 << '\n';
    return 0;
}
```
