---
title: Lemonade Line | USACO 2018 US Open Silver
date: 2020-08-22 22:50:00
category: USACO
tags:
- greedy
---

<!--more-->

```c++
#include <algorithm>
#include <deque>
#include <fstream>

std::ifstream fin("lemonade.in");
std::ofstream fout("lemonade.out");

int N;
std::deque<int> cows;
int num;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        int w;
        fin >> w;
        cows.push_back(w);
    }
    std::sort(cows.begin(), cows.end());
    while (!cows.empty())
    {
        int w = cows.front();
        cows.pop_front();
        if (num <= w)
            while (num <= w && !cows.empty())
                ++num, cows.pop_back();
        if (num <= w)
            ++num;
    }
    fout << num << '\n';
    return 0;
}
```

or

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("lemonade.in");
std::ofstream fout("lemonade.out");

int N;
int cows[MAX_N + 1];
int num;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> cows[n];
    std::sort(cows, cows + N);
    for (int n = N - 1; n >= 0; --n)
        if (num <= cows[n])
            ++num;
        else
            break;
    fout << num << '\n';
    return 0;
}
```
