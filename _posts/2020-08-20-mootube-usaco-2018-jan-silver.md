---
title: MooTube | USACO 2018 Jan Silver
date: 2020-08-20 22:15:00
category: USACO
tags:
- depth first search
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>
#include <vector>

#define MAX_N 5000
#define MAX_R 1000000000

std::ifstream fin("mootube.in");
std::ofstream fout("mootube.out");

int N, Q;
std::vector<int> edge[MAX_N + 1][2];
bool seen[MAX_N + 1];
int suggest;

void search(int k, int v, int r)
{
    if (r < k)
        return;
    seen[v] = true;
    ++suggest;
    for (int n = edge[v][0].size() - 1; n >= 0; --n)
        if (!seen[edge[v][0][n]])
            search(k, edge[v][0][n], std::min(edge[v][1][n], r));
}

int main()
{
    fin >> N >> Q;
    for (int n = 1; n < N; ++n)
    {
        int a, b, r;
        fin >> a >> b >> r;
        --a, --b;
        edge[a][0].push_back(b);
        edge[a][1].push_back(r);
        edge[b][0].push_back(a);
        edge[b][1].push_back(r);
    }
    for (int q = 0; q < Q; ++q)
    {
        int k, v;
        fin >> k >> v;
        --v;
        suggest = -1;
        for (int n = 0; n < N; ++n)
            seen[n] = false;
        search(k, v, MAX_R);
        fout << suggest << '\n';
    }
    return 0;
}
```
