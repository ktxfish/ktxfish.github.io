---
title: Wormhole Sort | USACO 2020 Jan Silver
date: 2020-09-01 19:30:00
category: USACO
tags:
- binary search
- flood fill
---

<!--more-->

```c++
#include <fstream>
#include <unordered_map>
#include <utility>
#include <vector>

#define MAX_N 100000
#define MAX_W 1000000000

std::ifstream fin("wormsort.in");
std::ofstream fout("wormsort.out");

int N, M, C;
std::vector<std::pair<int, int>> pair;
std::unordered_map<int, std::unordered_map<int, int>> edge; // start, (end, width)
int comp[MAX_N + 1];

void fill(int n, int w)
{
    comp[n] = C;
    for (auto ed : edge[n])
        if (ed.second >= w && comp[ed.first] == -1)
            fill(ed.first, w);
}

bool sorts(int w)
{
    C = 0;
    for (int n = 1; n <= N; ++n)
        comp[n] = -1;
    for (int n = 1; n <= N; ++n)
        if (comp[n] == -1)
            fill(n, w), ++C;
    for (auto pr : pair)
        if (comp[pr.first] != comp[pr.second])
            return false;
    return true;
}

int search(int beg, int end)
{
    if (beg == end)
        return beg;
    if (beg + 1 == end)
    {
        if (sorts(end))
            return end;
        return beg;
    }
    int mid = (beg + end) / 2;
    if (sorts(mid))
        return search(mid, end);
    return search(beg, mid - 1);
}

int main()
{
    fin >> N >> M;
    for (int n = 1; n <= N; ++n)
    {
        int p;
        fin >> p;
        if (n < p)
            pair.push_back({n, p});
    }
    if (pair.empty())
    {
        fout << "-1\n";
        return 0;
    }
    for (int m = 0; m < M; ++m)
    {
        int a, b, w;
        fin >> a >> b >> w;
        if (!(edge.count(a) && edge[a].count(b) && edge[a][b] >= w))
            edge[a][b] = w, edge[b][a] = w;
    }
    fout << search(1, MAX_W) << '\n';
    return 0;
}
```
