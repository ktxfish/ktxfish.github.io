---
title: USACO 2016 US Open Contest, Bronze
date: 2020-12-25
category: USACO
tags:
---

<!--more-->

# Diamond Collector

```c++
#include <fstream>

#define MAX_S 10000

std::ifstream fin("diamond.in");
std::ofstream fout("diamond.out");

int N, K;
int num[MAX_S + 1];
int max_show;

int main() {
    fin >> N >> K;
    for (int n = 0; n < N; ++n) {
        int s;
        fin >> s;
        ++num[s];
    }
    for (int s = 1; s <= MAX_S; ++s)
        num[s] += num[s - 1];
    for (int s = 1; s <= MAX_S - K; ++s) {
        int show = num[s + K] - num[s - 1];
        if (show > max_show)
            max_show = show;
    }
    fout << max_show << '\n';
    return 0;
}
```

# Bull in a China Shop

```c++
#include <fstream>

#define MAX_N 8
#define MAX_K 10

std::ifstream fin("bcs.in");
std::ofstream fout("bcs.out");

int N, K;
bool org[MAX_N][MAX_N];
bool pcs[MAX_K][MAX_N][MAX_N];
int top[MAX_K], btm[MAX_K], lft[MAX_K], rht[MAX_K];
int F, S;

int main() {
    fin >> N >> K;
    for (int n = 0; n < N; ++n) {
        fin.get();
        for (int m = 0; m < N; ++m) {
            char c;
            fin >> c;
            if (c == '#')
                org[n][m] = true;
        }
    }
    for (int k = 0; k < K; ++k) {
        for (int n = 0; n < N; ++n) {
            fin.get();
            for (int m = 0; m < N; ++m) {
                char c;
                fin >> c;
                if (c == '#')
                    pcs[k][n][m] = true;
            }
        }
        for (int n = 0; n < N; ++n)
            for (int m = 0; m < N; ++m)
                if (pcs[k][n][m]) {
                    top[k] = n;
                    goto end_top;
                }
        top[k] = N;
        end_top: ;
        for (int n = N - 1; n >= 0; --n)
            for (int m = 0; m < N; ++m)
                if (pcs[k][n][m]) {
                    btm[k] = N - n - 1;
                    goto end_btm;
                }
        btm[k] = N;
        end_btm: ;
        for (int m = 0; m < N; ++m)
            for (int n = 0; n < N; ++n)
                if (pcs[k][n][m]) {
                    lft[k] = m;
                    goto end_lft;
                }
        lft[k] = N;
        end_lft: ;
        for (int m = N - 1; m >= 0; --m)
            for (int n = 0; n < N; ++n)
                if (pcs[k][n][m]) {
                    rht[k] = N - m - 1;
                    goto end_rht;
                }
        rht[k] = N;
        end_rht: ;
    }
    for (int k = 0; k < K; ++k)
        for (int a = -top[k]; a <= btm[k]; ++a)
            for (int b = -lft[k]; b <= rht[k]; ++b) {
                bool first[MAX_N][MAX_N] = {};
                for (int n = 0; n < N; ++n)
                    for (int m = 0; m < N; ++m)
                        if (pcs[k][n][m])
                            first[n + a][m + b] = true;
                for (int h = k + 1; h < K; ++h)
                    for (int c = -top[h]; c <= btm[h]; ++c)
                        for (int d = -lft[h]; d <= rht[h]; ++d) {
                            bool second[MAX_N][MAX_N] = {};
                            for (int n = 0; n < N; ++n)
                                for (int m = 0; m < N; ++m)
                                    if (pcs[h][n][m])
                                        second[n + c][m + d] = true;
                            bool combo[MAX_N][MAX_N] = {};
                            for (int n = 0; n < N; ++n)
                                for (int m = 0; m < N; ++m)
                                    if (first[n][m] != second[n][m])
                                        combo[n][m] = true;
                            for (int n = 0; n < N; ++n)
                                for (int m = 0; m < N; ++m)
                                    if (combo[n][m] != org[n][m])
                                        goto next;
                            F = k;
                            S = h;
                            goto found;
                            next: ;
                        }
            }
    found: ;
    fout << F + 1 << ' ' << S + 1 << '\n';
    return 0;
}
```

Btw, I think the official solution is wrong… e.g. for this input

```
4 4
####
...#
....
....
##..
##..
##..
##..
..##
..##
..##
...#
...#
...#
....
....
###.
....
....
....
```

instead of

```
3 4
```

it gives the output

```
1 2
1 4
2 4
2 4
3 4
```

because it ignored the requirement stated in the problem that a piece
“cannot be shifted so far that any of its '#' characters fall outside
the original N×N grid”…

# Field Reduction

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 50000

std::ifstream fin("reduce.in");
std::ofstream fout("reduce.out");

int N;
struct Cow {
    int n, x, y;
} cow[MAX_N];
int min_area;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n) {
        cow[n].n = n;
        fin >> cow[n].x >> cow[n].y;
    }
    std::sort(cow, cow + N, [](const Cow a, const Cow b){ return a.x < b.x; });
    int min_x = cow[0].n;
    int min2_x = cow[1].n;
    int max_x = cow[N - 1].n;
    int max2_x = cow[N - 2].n;
    std::sort(cow, cow + N, [](const Cow a, const Cow b){ return a.y < b.y; });
    int min_y = cow[0].n;
    int min2_y = cow[1].n;
    int max_y = cow[N - 1].n;
    int max2_y = cow[N - 2].n;
    std::sort(cow, cow + N, [](const Cow a, const Cow b){ return a.n < b.n; });

    min_area = (cow[max_x].x - cow[min_x].x) * (cow[max_y].y - cow[min_y].y);
    if (min_x == min_y)
        min_area = std::min(min_area, (cow[max_x].x - cow[min2_x].x) * (cow[max_y].y - cow[min2_y].y));
    else if (min_x == max_y)
        min_area = std::min(min_area, (cow[max_x].x - cow[min2_x].x) * (cow[max2_y].y - cow[min_y].y));
    else
        min_area = std::min(min_area, (cow[max_x].x - cow[min2_x].x) * (cow[max_y].y - cow[min_y].y));
    if (max_x == min_y)
        min_area = std::min(min_area, (cow[max2_x].x - cow[min_x].x) * (cow[max_y].y - cow[min2_y].y));
    else if (max_x == max_y)
        min_area = std::min(min_area, (cow[max2_x].x - cow[min_x].x) * (cow[max2_y].y - cow[min_y].y));
    else
        min_area = std::min(min_area, (cow[max2_x].x - cow[min_x].x) * (cow[max_y].y - cow[min_y].y));
    if (min_y != min_x && min_y != max_x)
        min_area = std::min(min_area, (cow[max_x].x - cow[min_x].x) * (cow[max_y].y - cow[min2_y].y));
    if (max_y != min_x && max_y != max_x)
        min_area = std::min(min_area, (cow[max_x].x - cow[min_x].x) * (cow[max2_y].y - cow[min_y].y));
    fout << min_area << '\n';
    return 0;
}
```

Maybe you can avoid that repetitive blob by actually removing the cows, but I'm
too lazy to think.
