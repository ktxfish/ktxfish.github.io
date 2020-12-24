---
title: USACO 2016 January Contest, Bronze
date: 2020-12-24
category: USACO
tags:
---

<!--more-->

# Promotion Counting

```c++
#include <fstream>

std::ifstream fin("promote.in");
std::ofstream fout("promote.out");

int reg[4][2], pro[4];

int main() {
    fin >> reg[0][0] >> reg[0][1] >> reg[1][0] >> reg[1][1]
        >> reg[2][0] >> reg[2][1] >> reg[3][0] >> reg[3][1];
    for (int p = 3; p > 0; --p) {
        pro[p] = reg[p][1] - reg[p][0];
        reg[p - 1][1] += pro[p];
    }
    for (int p = 1; p < 4; ++p)
        fout << pro[p] << '\n';
    return 0;
}
```

# Angry Cows

Only need to simulate the leftmost and rightmost haybales.

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100

std::ifstream fin("angry.in");
std::ofstream fout("angry.out");

int N;
int loc[MAX_N];
int max_x;

int get_right(int n, int t) {
    int i = n + 1;
    while (i < N && loc[i] - loc[n] <= t)
        ++i;
    if (i == n + 1)
        return n;
    return get_right(i - 1, t + 1);
}
int get_left(int n, int t) {
    int i = n - 1;
    while (i >= 0 && loc[n] - loc[i] <= t)
        --i;
    if (i == n - 1)
        return n;
    return get_left(i + 1, t + 1);
}

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> loc[n];
    std::sort(loc, loc + N);

    for (int n = 0; n < N; ++n) {
        int x = get_right(n, 1) - get_left(n, 1) + 1;
        if (x > max_x)
            max_x = x;
    }
    fout << max_x << '\n';
    return 0;
}
```

# Mowing the Field

```c++
#include <fstream>

#define MAX_N 100
#define MAX_S 10

std::ifstream fin("mowing.in");
std::ofstream fout("mowing.out");

int N;
int field[MAX_N * MAX_S * 2 + 1][MAX_N * MAX_S * 2 + 1];
int x = MAX_N * MAX_S + 1;

int main() {
    fin >> N;

    int h = MAX_N * MAX_S, k = MAX_N * MAX_S;
    field[h][k] = 1;

    for (int n = 0, t = 2; n < N; ++n) {
        char d;
        int S;
        fin >> d >> S;

        int a = 0, b = 0;
        if (d == 'N')
            a = -1;
        else if (d == 'E')
            b = 1;
        else if (d == 'S')
            a = 1;
        else
            b = -1;

        for (int s = 0; s < S; ++s, ++t) {
            h += a;
            k += b;
            if (field[h][k] > 0 && t - field[h][k] < x)
                x = t - field[h][k];
            field[h][k] = t;
        }
    }
    if (x == MAX_N * MAX_S + 1)
        fout << -1 << '\n';
    else
        fout << x << '\n';
    return 0;
}
```
