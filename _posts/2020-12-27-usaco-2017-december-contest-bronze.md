---
title: USACO 2017 December Contest, Bronze
date: 2020-12-27 16:30:00
category: USACO
tags:
---

<!--more-->

# Blocked Billboard

```c++
#include <algorithm>
#include <fstream>

std::ifstream fin("billboard.in");
std::ofstream fout("billboard.out");

int b1x1, b1y1, b1x2, b1y2;
int b2x1, b2y1, b2x2, b2y2;
int tx1, ty1, tx2, ty2;
int ans;

int main() {
    fin >> b1x1 >> b1y1 >> b1x2 >> b1y2
        >> b2x1 >> b2y1 >> b2x2 >> b2y2
        >> tx1 >> ty1 >> tx2 >> ty2;
    ans = (b1x2 - b1x1) * (b1y2 - b1y1) + (b2x2 - b2x1) * (b2y2 - b2y1);
    int a = std::min(tx2, b1x2) - std::max(tx1, b1x1);
    int b = std::min(ty2, b1y2) - std::max(ty1, b1y1);
    if (a > 0 && b > 0)
        ans -= a * b;
    a = std::min(tx2, b2x2) - std::max(tx1, b2x1);
    b = std::min(ty2, b2y2) - std::max(ty1, b2y1);
    if (a > 0 && b > 0)
        ans -= a * b;
    fout << ans << '\n';
    return 0;
}
```

# The Bovine Shuffle

```c++
#include <fstream>

#define MAX_N 100

std::ifstream fin("shuffle.in");
std::ofstream fout("shuffle.out");

int N;
int order[MAX_N];
int back[MAX_N];

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n) {
        int b;
        fin >> b;
        back[b - 1] = n;
    }
    for (int n = 0; n < N; ++n)
        fin >> order[n];
    for (int i = 0; i < 3; ++i) {
        int new_order[MAX_N];
        for (int n = 0; n < N; ++n)
            new_order[back[n]] = order[n];
        for (int n = 0; n < N; ++n)
            order[n] = new_order[n];
    }
    for (int n = 0; n < N; ++n)
        fout << order[n] << '\n';
    return 0;
}
```

# Milk Measurement

```c++
#include <algorithm>
#include <fstream>
#include <string>

#define MAX_N 100

std::ifstream fin("measurement.in");
std::ofstream fout("measurement.out");

int N;
struct Rec {
    int day, who, amt;
} rec[MAX_N];
int milk[3] = {7, 7, 7};
bool on[3] = {true, true, true};
int adj;

int idx(std::string name) {
    if (name == "Bessie")
        return 0;
    if (name == "Elsie")
        return 1;
    return 2;
}

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n) {
        std::string name;
        fin >> rec[n].day >> name >> rec[n].amt;
        rec[n].who = idx(name);
    }
    std::sort(rec, rec + N, [](Rec a, Rec b){ return a.day < b.day; });
    for (int n = 0; n < N; ++n) {
        milk[rec[n].who] += rec[n].amt;
        int max_amt = 0;
        for (int i = 0; i < 3; ++i)
            if (milk[i] > max_amt)
                max_amt = milk[i];

        bool need_adj = false;
        for (int i = 0; i < 3; ++i)
            if (milk[i] == max_amt) {
                if (!on[i])
                    need_adj = true, on[i] = true;
            } else if (on[i]) {
                need_adj = true, on[i] = false;
            }
        if (need_adj)
            ++adj;
    }
    fout << adj << '\n';
    return 0;
}
```
