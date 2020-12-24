---
title: USACO 2015 December Contest, Bronze
date: 2020-12-24 14:30:00
category: USACO
tags:
---

<!--more-->

# Fence Painting

Ran over the different cases:

```c++
#include <fstream>

std::ifstream fin("paint.in");
std::ofstream fout("paint.out");

int main() {
    int a, b, c, d;
    fin >> a >> b >> c >> d;
    int ans = b - a + d - c;
    if (c >= a && d <= b)
        ans -= d - c;
    else if (a >= c && b <= d)
        ans -= b - a;
    else if (c < a && a < d)
        ans -= d - a;
    else if (c < b && b < d)
        ans -= b - c;
    fout << ans << '\n';
    return 0;
}
```

Can also keep an array as at there are at most 100 fences.

# Speeding Ticket

Compared the segments:

```c++
#include <fstream>

#define MAX_LEN 100

std::ifstream fin("speeding.in");
std::ofstream fout("speeding.out");

int N, M;
struct Seg {
    int beg, end, spd;
} road[MAX_LEN], cow[MAX_LEN];
int max_over;

int main() {
    fin >> N >> M;
    fin >> road[0].end >> road[0].spd;
    road[0].beg = 0;
    for (int n = 1; n < N; ++n) {
        road[n].beg = road[n - 1].end;
        int len, spd;
        fin >> len >> spd;
        road[n].end = road[n].beg + len;
        road[n].spd = spd;
    }
    fin >> cow[0].end >> cow[0].spd;
    cow[0].beg = 0;
    for (int m = 1; m < M; ++m) {
        cow[m].beg = cow[m - 1].end;
        int len, spd;
        fin >> len >> spd;
        cow[m].end = cow[m].beg + len;
        cow[m].spd = spd;
    }
    for (int n = 1, m = 0; m < M; ++m) {
        if (road[n - 1].end > cow[m].beg &&
            road[n - 1].spd < cow[m].spd &&
            cow[m].spd - road[n - 1].spd > max_over)
            max_over = cow[m].spd - road[n - 1].spd;
        while (n < N && road[n].beg < cow[m].end) {
            if (road[n].spd < cow[m].spd &&
                cow[m].spd - road[n].spd > max_over)
                max_over = cow[m].spd - road[n].spd;
            ++n;
        }
    }
    fout << max_over << '\n';
    return 0;
}
```

Can also keep two arrays, one for speed limit and one for Bessie's speed, and
compare every element as there are at most 100 road segments.

# Contaminated Milk

Run over the transcript, keeping the first time a friend drinks any type of
milk and the total number of friends that drank any type of milk. If a certain
type of milk is bad, then everyone who became sick must have drank it before
they got sick. Run over the types of milk that were drank by all the sick people
and prepare doeses for the type with the most total number of people who drank it.

```c++
#include <fstream>

#define MAX_N 50
#define MAX_M 50

std::ifstream fin("badmilk.in");
std::ofstream fout("badmilk.out");

int N, M, D, S;
int first[MAX_N][MAX_M];
int bad[MAX_M];
int ppl[MAX_M];
int med;

int main() {
    fin >> N >> M >> D >> S;
    for (int d = 0; d < D; ++d) {
        int p, m, t;
        fin >> p >> m >> t;
        --p, --m;
        if (first[p][m] == 0)
            ++ppl[m];
        if (first[p][m] == 0 || t < first[p][m])
            first[p][m] = t;
    }
    for (int s = 0; s < S; ++s) {
        int p, t;
        fin >> p >> t;
        --p;
        for (int m = 0; m < M; ++m)
            if (first[p][m] != 0 && first[p][m] < t)
                ++bad[m];
    }
    for (int m = 0; m < M; ++m)
        if (bad[m] == S && ppl[m] > med)
            med = ppl[m];
    fout << med << '\n';
    return 0;
}
```
