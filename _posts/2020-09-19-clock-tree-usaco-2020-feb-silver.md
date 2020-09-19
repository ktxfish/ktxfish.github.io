---
title: Clock Tree | USACO 2020 Feb Silver
date: 2020-09-19 15:30:00
category: USACO
tags:
- greedy
---

<!--more-->

```c++
#include <fstream>
#include <vector>

#define MAX_N 2500

std::ifstream fin("clocktree.in");
std::ofstream fout("clocktree.out");

int N;
int ini[MAX_N], now[MAX_N];
std::vector<int> edge[MAX_N];
bool seen[MAX_N];
int ans;

void search(int i)
{
    seen[i] = true;
    for (int j : edge[i])
        if (!seen[j])
            search(j);
    for (int j : edge[i])
        now[i] += 12 - now[j], now[j] = 0;
    now[i] %= 12;
}

int main()
{
    fin >> N;
    for (int i = 0; i < N; ++i)
        fin >> ini[i], ini[i] %= 12;
    for (int i = 1; i < N; ++i)
    {
        int a, b;
        fin >> a >> b;
        edge[a - 1].push_back(b - 1);
        edge[b - 1].push_back(a - 1);
    }
    for (int i = 0; i < N; ++i)
    {
        for (int j = 0; j < N; ++j)
            now[j] = ini[j], seen[j] = false;
        search(i);
        if (now[i] == 0 || now[i] == 1)
            ++ans;
    }
    fout << ans << '\n';
    return 0;
}
```
