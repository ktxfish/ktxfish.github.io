---
title: USACO 2018 February Contest, Bronze
date: 2020-12-30 16:40:00
category: USACO
tags:
---

<!--more-->

# Teleportation

```c++
#include <algorithm>
#include <fstream>

std::ifstream fin("teleport.in");
std::ofstream fout("teleport.out");

int a, b, x, y;

int main() {
    fin >> a >> b >> x >> y;
    fout << std::min({
        std::abs(a - b),
        std::abs(a - x) + std::abs(y - b),
        std::abs(a - y) + std::abs(x - b)
        }) << '\n';
    return 0;
}
```

# Hoofball

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100

std::ifstream fin("hoofball.in");
std::ofstream fout("hoofball.out");

int N;
int loc[MAX_N];
int pass[MAX_N];
int num_init;

int target(int i) {
    if (i == N - 1 || (i != 0 &&
        loc[i] - loc[i - 1] <= loc[i + 1] - loc[i]))
        return i - 1;
    return i + 1;
}

int main() {
    fin >> N;
    if (N == 1) {
        fout << "1\n";
        return 0;
    }
    for (int n = 0; n < N; ++n)
        fin >> loc[n];
    std::sort(loc, loc + N);

    for (int n = 0; n < N; ++n) {
        bool seen[MAX_N] = {};
        int i = n;
        while (!seen[i]) {
            seen[i] = true;
            i = target(i);
        }
        for (int m = 0; m < N; ++m)
            if (seen[m])
                ++pass[m];
        --pass[n];
    }
    for (int n = 0; n < N; ++n)
        if (pass[n] == 0)
            ++num_init;
        else if (n != N - 1 && target(n) == n + 1 && target(n + 1) == n
            && pass[n] == 1 && pass[target(n)] == 1)
            ++num_init;
    fout << num_init << '\n';
    return 0;
}
```

# Taming the Herd

```c++
#include <fstream>

#define MAX_N 100

std::ifstream fin("taming.in");
std::ofstream fout("taming.out");

int N;
int counter[MAX_N];
int breakout[MAX_N];
int min, maybe;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> counter[n];
    counter[0] = 0;

    for (int n = 0; n < N; ++n) {
        if (counter[n] == -1)
            continue;

        if (breakout[n - counter[n]] == -1) {
            fout << "-1\n";
            return 0;
        }
        breakout[n - counter[n]] = 1;

        if (counter[n] > 0)
            for (int i = n; i > n - counter[n]; --i) {
                if (breakout[i] == 1) {
                    fout << "-1\n";
                    return 0;
                }
                breakout[i] = -1;
            }
    }
    for (int n = 0; n < N; ++n)
        if (breakout[n] == 1)
            ++min;
        else if (breakout[n] == 0)
            ++maybe;
    fout << min << ' ' << min + maybe << '\n';
    return 0;
}
```
