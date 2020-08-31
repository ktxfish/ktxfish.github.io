---
title: Berry Picking | USACO 2020 Jan Silver
date: 2020-08-30 21:50:00
category: USACO
tags:
- greedy
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>
#include <queue>

#define MAX_N 1000

std::ifstream fin("berries.in");
std::ofstream fout("berries.out");

int N, K;
int B[MAX_N], max_b, max_col;

int main()
{
    fin >> N >> K;
    for (int n = 0; n < N; ++n)
    {
        fin >> B[n];
        if (B[n] > max_b)
            max_b = B[n];
    }
    for (int b = 1; b < max_b; ++b)
    {
        int amt = 0;
        std::priority_queue<int> rest;
        for (int n = 0; n < N; ++n)
        {
            amt += B[n] / b;
            rest.push(B[n] % b);
        }
        int col = std::min(K / 2, (amt - K / 2)) * b;
        for (int l = K - amt; l > 0 && !rest.empty(); --l)
        {
            col += rest.top();
            rest.pop();
        }
        if (col > max_col)
            max_col = col;
    }
    fout << max_col << '\n';
    return 0;
}
```
