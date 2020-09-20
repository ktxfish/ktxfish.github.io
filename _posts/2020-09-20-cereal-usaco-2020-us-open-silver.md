---
title: Cereal | USACO 2020 US Open Silver
date: 2020-09-20 14:45:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100000
#define MAX_M 100000

std::ifstream fin("cereal.in");
std::ofstream fout("cereal.out");

int N, M;
int F[MAX_N], S[MAX_N];
int idx[MAX_M + 1];
int ans[MAX_N];

void update(int i, int m, int j, int n)
{
    if (j != -1 && j < i)
    {
        ans[n] = ans[n + 1];
        return;
    }
    idx[m] = i;
    if (j == -1)
    {
        ans[n] = ans[n + 1] + 1;
        return;
    }
    if (S[j] == m)
    {
        ans[n] = ans[n + 1];
        return;
    }
    update(j, S[j], idx[S[j]], n);
}

int main()
{
    fin >> N >> M;
    for (int i = 0; i < N; ++i)
        fin >> F[i] >> S[i];
    for (int i = 1; i <= M; ++i)
        idx[i] = -1;
    idx[F[N - 1]] = N - 1;
    ans[N - 1] = 1;
    for (int i = N - 2; i >= 0; --i)
        update(i, F[i], idx[F[i]], i);
    for (int i = 0; i < N; ++i)
        fout << ans[i] << '\n';
    return 0;
}
```
