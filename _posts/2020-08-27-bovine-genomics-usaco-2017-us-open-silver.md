---
title: Bovine Genomics | USACO 2017 US Open Silver
date: 2020-08-27 13:55:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 500
#define MAX_M 50

std::ifstream fin("cownomics.in");
std::ofstream fout("cownomics.out");

int N, M;
char spotty[MAX_N][MAX_M + 1], plain[MAX_N][MAX_M + 1];
bool cause[4][4][4];
int ans;

int idx(char l)
{
    int i;
    switch (l)
    {
    case 'A':
        i = 0;
        break;
    case 'C':
        i = 1;
        break;
    case 'G':
        i = 2;
        break;
    case 'T':
        i = 3;
        break;
    }
    return i;
}

int main()
{
    fin >> N >> M;
    for (int n = 0; n < N; ++n)
        fin >> spotty[n];
    for (int n = 0; n < N; ++n)
        fin >> plain[n];
    for (int a = 0; a < M; ++a)
        for (int b = a + 1; b < M; ++b)
            for (int c = b + 1; c < M; ++c)
            {
                for (int i = 0; i < 4; ++i)
                    for (int j = 0; j < 4; ++j)
                        for (int k = 0; k < 4; ++k)
                            cause[i][j][k] = false;
                for (int n = 0; n < N; ++n)
                    cause[idx(spotty[n][a])][idx(spotty[n][b])][idx(spotty[n][c])] = true;
                for (int n = 0; n < N; ++n)
                    if (cause[idx(plain[n][a])][idx(plain[n][b])][idx(plain[n][c])])
                        goto next_combo;
                ++ans;
                next_combo: ;
            }
    fout << ans << '\n';
    return 0;
}
```
