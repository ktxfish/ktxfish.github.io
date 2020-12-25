---
title: USACO 2016 February Contest, Bronze
date: 2020-12-25 12:15:00
category: USACO
tags:
---

<!--more-->

# Milk Pails

```c++
#include <fstream>

std::ifstream fin("pails.in");
std::ofstream fout("pails.out");

int X, Y, M;
int max_amt;

int main() {
    fin >> X >> Y >> M;
    for (int y = 0; y <= M / Y; ++y) {
        int amt = y * Y + (M - y * Y) / X * X;
        if (amt > max_amt)
            max_amt = amt;
    }
    fout << max_amt << '\n';
    return 0;
}
```

# Circular Barn

```c++
#include <fstream>

#define MAX_N 1000
#define MAX_R 100

std::ifstream fin("cbarn.in");
std::ofstream fout("cbarn.out");

int N;
int r[MAX_N];
int min_walk = MAX_N * MAX_N * MAX_R;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> r[n];

    for (int n = 0; n < N; ++n) {
        int walk = 0;
        for (int i = 1; i < N; ++i)
            walk += i * r[(n + i) % N];
        if (walk < min_walk)
            min_walk = walk;
    }
    fout << min_walk << '\n';
    return 0;
}
```

# Load Balancing

```c++
#include <algorithm>
#include <fstream>
#include <set>

#define MAX_N 100

std::ifstream fin("balancing.in");
std::ofstream fout("balancing.out");

int N, B;
struct Cow {
    int x, y;
} cow[MAX_N];
std::set<int> xcord, ycord;
int M = MAX_N;

int main() {
    fin >> N >> B;
    for (int n = 0; n < N; ++n) {
        fin >> cow[n].x >> cow[n].y;
        xcord.insert(cow[n].x);
        ycord.insert(cow[n].y);
    }
    for (int x : xcord)
        for (int y : ycord) {
            int a = 0, b = 0, c = 0, d = 0;
            for (int n = 0; n < N; ++n)
                if (cow[n].x <= x && cow[n].y <= y)
                    ++a;
                else if (cow[n].x <= x && cow[n].y > y)
                    ++b;
                else if (cow[n].x > x && cow[n].y <= y)
                    ++c;
                else
                    ++d;
            int m = std::max({a, b, c, d});
            if (m < M)
                M = m;
        }
    fout << M << '\n';
    return 0;
}
```
