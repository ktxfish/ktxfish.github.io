---
title: Why Did the Cow Cross the Road II | USACO 2017 Feb Silver
date: 2020-08-25 18:00:00
category: USACO
tags:
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("maxcross.in");
std::ofstream fout("maxcross.out");

int N, K, B;
int repair[MAX_N], min_repair = MAX_N;

int main()
{
    fin >> N >> K >> B;
    for (int b = 0; b < B; ++b)
    {
        int id;
        fin >> id;
        for (int r = std::max(0, id - K); r < id; ++r)
            ++repair[r];
    }
    for (int r = 0; r <= N - K; ++r)
        if (repair[r] < min_repair)
            min_repair = repair[r];
    fout << min_repair << '\n';
    return 0;
}
```
