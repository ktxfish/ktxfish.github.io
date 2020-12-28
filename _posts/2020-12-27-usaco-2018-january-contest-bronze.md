---
title: USACO 2018 January Contest, Bronze
date: 2020-12-27 21:15:00
category: USACO
tags:
---

<!--more-->

# Blocked Billboard II

```c++
#include <algorithm>
#include <fstream>

std::ifstream fin("billboard.in");
std::ofstream fout("billboard.out");

int lx1, ly1, lx2, ly2;
int fx1, fy1, fx2, fy2;
int tarp;

int main() {
    fin >> lx1 >> ly1 >> lx2 >> ly2
        >> fx1 >> fy1 >> fx2 >> fy2;
    tarp = (lx2 - lx1) * (ly2 - ly1);

    if (fx1 <= lx1 && fx2 >= lx2 && (fy1 <= ly1 || fy2 >= ly2)) {
        int a = std::min(fy2, ly2) - std::max(fy1, ly1);
        if (a > 0)
            tarp -= (lx2 - lx1) * a;
    } else if (fy1 <= ly1 && fy2 >= ly2 && (fx1 <= lx1 || fx2 >= lx2)) {
        int b = std::min(fx2, lx2) - std::max(fx1, lx1);
        if (b > 0)
            tarp -= (ly2 - ly1) * b;
    }
    fout << tarp << '\n';
    return 0;
}
```

# Lifeguards

```c++
#include <fstream>

#define MAX_N 100
#define MAX_T 1000

std::ifstream fin("lifeguards.in");
std::ofstream fout("lifeguards.out");

int N;
struct Shift {
    int beg, end;
} shift[MAX_N];
int cover[MAX_T + 1];
int ttl_cover;
int min_lost = MAX_T;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> shift[n].beg >> shift[n].end;

    for (int n = 0; n < N; ++n)
        for (int t = shift[n].beg; t < shift[n].end; ++t)
            ++cover[t];
    for (int t = 0; t <= MAX_T; ++t)
        if (cover[t] > 0)
            ++ttl_cover;
    for (int n = 0; n < N; ++n) {
        int lost = 0;
        for (int t = shift[n].beg; t < shift[n].end; ++t)
            if (cover[t] == 1)
                ++lost;
        if (lost < min_lost)
            min_lost = lost;
    }
    fout << ttl_cover - min_lost << '\n';
    return 0;
}
```

# Out of Place

First did this:

```c++
#include <fstream>

#define MAX_N 100

std::ifstream fin("outofplace.in");
std::ofstream fout("outofplace.out");

int N;
int ord[MAX_N];
int bes;
int swap;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> ord[n];

    for (int n = 1; n < N - 1; ++n)
        if (ord[n] > ord[n + 1] && (n >= N - 2 || ord[n + 2] < ord[n])
            || ord[n] < ord[n - 1]) {
            bes = n;
            break;
        }
    if (ord[N - 1] < ord[N - 2])
        bes = N - 1;
    if (bes > 0 && ord[bes] < ord[bes - 1]) {
        int n = bes;
        while (n > 0 && ord[bes] < ord[n - 1]) {
            ++swap;
            do {
                --n;
            } while (n > 0 && ord[n - 1] == ord[n]);
        }
    } else {
        int n = bes;
        while (n < N - 1 && ord[bes] > ord[n + 1]) {
            ++swap;
            do {
                ++n;
            } while (n < N - 1 && ord[n + 1] == ord[n]);
        }
    }
    fout << swap << '\n';
    return 0;
}
```

But can be done much easier with this:

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100

std::ifstream fin("outofplace.in");
std::ofstream fout("outofplace.out");

int N;
int height[MAX_N], sorted[MAX_N];
int swap = -1;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> height[n], sorted[n] = height[n];
    std::sort(sorted, sorted + N);
    for (int i = 0; i < N; ++i)
        if (sorted[i] != height[i])
            ++swap;
    fout << std::max(0, swap) << '\n';
    return 0;
}
```
