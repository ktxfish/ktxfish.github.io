---
title: USACO 2017 US Open Contest, Bronze
date: 2020-12-26 23:40:00
category: USACO
tags:
---

<!--more-->

# The Lost Cow

```c++
#include <fstream>

std::ifstream fin("lostcow.in");
std::ofstream fout("lostcow.out");

int x, y;
const int exp[] = {
    1, 2, 4, 8, 16, 32, 64, 128, 256,
    512, 1024, 2048, 4096, 8192
    };

int main() {
    fin >> x >> y;
    int z = x, d = 1;
    for (int i = 1; x < y ? z < y : z > y; ++i)
        d += exp[i - 1] + exp[i],
        z = x + (i % 2 == 0 ? 1 : -1) * exp[i];
    d -= (x < y ? z - y : y - z);
    fout << d << '\n';
    return 0;
}
```

# Bovine Genomics

```c++
#include <fstream>

#define MAX_N 100
#define MAX_M 100

std::ifstream fin("cownomics.in");
std::ofstream fout("cownomics.out");

int N, M;
char spotty[MAX_N][MAX_M + 1], plain[MAX_N][MAX_M + 1];
int count;

int main() {
    fin >> N >> M;
    for (int n = 0; n < N; ++n)
        fin >> spotty[n];
    for (int n = 0; n < N; ++n)
        fin >> plain[n];
    for (int m = 0; m < M; ++m) {
        bool spot[26] = {};
        for (int n = 0; n < N; ++n)
            spot[spotty[n][m] - 'A'] = true;
        for (int n = 0; n < N; ++n)
            if (spot[plain[n][m] - 'A'])
                goto next_m;
        ++count;
        next_m: ;
    }
    fout << count << '\n';
    return 0;
}
```

# Modern Art

```c++
#include <fstream>

#define MAX_N 10

std::ifstream fin("art.in");
std::ofstream fout("art.out");

int N;
char art[MAX_N][MAX_N + 1];
bool can[10];
int num_can;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> art[n];
    for (int n = 0; n < N; ++n)
        for (int m = 0; m < N; ++m)
            can[art[n][m] - '0'] = true;
    for (int c = 1; c <= 9; ++c) {
        int x1 = N, y1 = N, x2 = -1, y2 = -1;
        for (int n = 0; n < N; ++n)
            for (int m = 0; m < N; ++m)
                if (art[n][m] - '0' == c) {
                    if (n < x1)
                        x1 = n;
                    if (m < y1)
                        y1 = m;
                    if (n > x2)
                        x2 = n;
                    if (m > y2)
                        y2 = m;
                }
        if (x1 == N)
            continue;
        for (int n = x1; n <= x2; ++n)
            for (int m = y1; m <= y2; ++m)
                if (art[n][m] - '0' != c)
                    can[art[n][m] - '0'] = false;
    }
    for (int c = 1; c <= 9; ++c)
        if (can[c])
            ++num_can;
    fout << num_can << '\n';
    return 0;
}
```
