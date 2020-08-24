---
title: Sleepy Cow Herding | USACO 2019 Feb Silver
date: 2020-08-16 11:15::00
category: USACO
tags:
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("herding.in");
std::ofstream fout("herding.out");

int N;
int loc[MAX_N];
int min_moves = MAX_N;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> loc[n];
    std::sort(loc, loc + N);
    if ((loc[N - 2] - loc[0] == N - 2 && loc[N - 1] - loc[N - 2] > 2) ||
        (loc[N - 1] - loc[1] == N - 2 && loc[1] - loc[0] > 2))
        min_moves = 2;
    else
        for (int n = 0; n < N; ++n)
        {
            int m = n + 1;
            while (m < N && loc[m] - loc[n] < N)
                ++m;
            if (N - (m - n) < min_moves)
                min_moves = N - (m - n);
        }
    fout << min_moves << '\n'
         << std::max(loc[N - 2] - loc[0], loc[N - 1] - loc[1]) - N + 2 << '\n';
    return 0;
}
```
