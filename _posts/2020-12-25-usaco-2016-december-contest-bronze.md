---
title: USACO 2016 December Contest, Bronze
date: 2020-12-25 22:15:00
category: USACO
tags:
---

<!--more-->

# Square Pasture

```c++
#include <algorithm>
#include <fstream>

std::ifstream fin("square.in");
std::ofstream fout("square.out");

int ax1, ay1, ax2, ay2;
int bx1, by1, bx2, by2;

int main() {
    fin >> ax1 >> ay1 >> ax2 >> ay2
        >> bx1 >> by1 >> bx2 >> by2;
    int s = std::max({
        ax2 - bx1, bx2 - ax1,
        ay2 - by1, by2 - ay1,
        ax2 - ax1, bx2 - bx1,
        ay2 - ay1, by2 - by1
        });
    fout << s * s << '\n';
    return 0;
}
```

# Block Game

```c++
#include <algorithm>
#include <fstream>

std::ifstream fin("blocks.in");
std::ofstream fout("blocks.out");

int N;
int ttl[26];

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)  {
        char word[11];
        fin >> word;
        int cnt1[26] = {};
        for (int c = 0; c < 11 && word[c]; ++c)
            ++cnt1[word[c] - 'a'];
        fin >> word;
        int cnt2[26] = {};
        for (int c = 0; c < 11 && word[c]; ++c)
            ++cnt2[word[c] - 'a'];
        for (int i = 0; i < 26; ++i)
            ttl[i] += std::max(cnt1[i], cnt2[i]);
    }
    for (int i = 0; i < 26; ++i)
        fout << ttl[i] << '\n';
    return 0;
}
```

# The Cow-Signal

```c++
#include <fstream>

#define MAX_M 10
#define MAX_N 10

std::ifstream fin("cowsignal.in");
std::ofstream fout("cowsignal.out");

int M, N, K;
char sig[MAX_M][MAX_N + 1];

int main() {
    fin >> M >> N >> K;
    for (int m = 0; m < M; ++m)
        fin >> sig[m];
    for (int m = 0; m < M; ++m)
        for (int k = 0; k < K; ++k) {
            for (int n = 0; n < N; ++n)
                for (int h = 0; h < K; ++h)
                    fout << sig[m][n];
            fout << '\n';
        }
    return 0;
}
```
