---
title: High Card Wins | USACO 2015 Dec Silver
date: 2020-08-28 21:15:00
category: USACO
tags:
- greedy
---

<!--more-->

```c++
#include <fstream>
#include <functional>
#include <queue>

#define MAX_N 50000

std::ifstream fin("highcard.in");
std::ofstream fout("highcard.out");

int N;
bool own[MAX_N * 2];
std::priority_queue<int, std::vector<int>, std::greater<int>> elsie, bessie;
int points;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        int v;
        fin >> v;
        own[v - 1] = true;
    }
    for (int n = 0; n < N * 2; ++n)
        if (own[n])
            elsie.push(n);
        else
            bessie.push(n);
    while (!bessie.empty())
    {
        int f = elsie.top(), g = bessie.top();
        elsie.pop(), bessie.pop();
        while (g < f && !bessie.empty())
            g = bessie.top(), bessie.pop();
        if (g > f)
            ++points;
    }
    fout << points << '\n';
    return 0;
}
```
