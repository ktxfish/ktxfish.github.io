---
title: Out of Sorts | USACO 2018 US Open Silver
date: 2020-08-22 20:00:00
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

std::ifstream fin("sort.in");
std::ofstream fout("sort.out");

int N;
std::pair<int, int> A[MAX_N + 1];
int moo;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> A[n].first, A[n].second = n;
    std::sort(A, A + N);
    for (int n = 0; n < N; ++n)
        if (A[n].second - n > moo)
            moo = A[n].second - n;
    fout << moo + 1 << '\n';
    return 0;
}
```
