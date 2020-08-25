---
title: Why Did the Cow Cross the Road II | USACO 2017 Feb Silver
date: 2020-08-25 18:00:00
category: USACO
tags:
- prefix sum
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100000

std::ifstream fin("maxcross.in");
std::ofstream fout("maxcross.out");

int N, K, B;
int repair[MAX_N + 1], min_repair = MAX_N;

int main()
{
    fin >> N >> K >> B;
    for (int b = 0; b < B; ++b)
    {
        int id;
        fin >> id;
        ++repair[id];
    }
    for (int n = 1; n <= N; ++n)
        repair[n] += repair[n - 1];
    for (int n = 0; n <= N - K; ++n)
        if (repair[n + K] - repair[n] < min_repair)
            min_repair = repair[n + K] - repair[n];
    fout << min_repair << '\n';
    return 0;
}
```
