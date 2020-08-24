---
title: Multiplayer Moo | USACO 2018 US Open Silver
date: 2020-08-23 22:30:00
category: USACO
tags:
- depth first search
- flood fill
- map
- set
---

<!--more-->

```c++
#include <fstream>
#include <map>
#include <set>
#include <utility>

#define MAX_N 250

std::ifstream fin("multimoo.in");
std::ofstream fout("multimoo.out");

struct Cell
{
    int id;
    int room = -1;
};
struct Node
{
    int id;
    int size = 0;
    std::set<int> adj;
};

int N, R, max_single_size, max_double_size, double_size;
Cell cell[MAX_N][MAX_N];
Node node[MAX_N * MAX_N];
std::map<std::pair<int, int>, bool> seen_edge;
std::map<int, bool> seen_node;

void single_search(int n, int m)
{
    cell[n][m].room = R;
    ++node[R].size;
    if (n && cell[n - 1][m].id == cell[n][m].id && cell[n - 1][m].room == -1)
        single_search(n - 1, m);
    if (m && cell[n][m - 1].id == cell[n][m].id && cell[n][m - 1].room == -1)
        single_search(n, m - 1);
    if (n + 1 < N && cell[n + 1][m].id == cell[n][m].id && cell[n + 1][m].room == -1)
        single_search(n + 1, m);
    if (m + 1 < N && cell[n][m + 1].id == cell[n][m].id && cell[n][m + 1].room == -1)
        single_search(n, m + 1);
}
void double_search(int a, int b, int r)
{
    seen_node[r] = true;
    double_size += node[r].size;
    int t = (node[r].id == a ? b : a);
    for (int s : node[r].adj)
        if (node[s].id == t)
        {
            seen_edge[{r, s}] = true, seen_edge[{s, r}] = true;
            if (!seen_node.count(s))
                double_search(a, b, s);
        }
}

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        for (int m = 0; m < N; ++m)
            fin >> cell[n][m].id;
    for (int n = 0; n < N; ++n)
        for (int m = 0; m < N; ++m)
            if (cell[n][m].room == -1)
            {
                node[R].id = cell[n][m].id;
                single_search(n, m);
                if (node[R].size > max_single_size)
                    max_single_size = node[R].size;
                ++R;
            }
    fout << max_single_size << '\n';
    for (int n = 0; n < N; ++n)
        for (int m = 0; m < N; ++m)
        {
            if (n && cell[n - 1][m].id != cell[n][m].id)
                node[cell[n][m].room].adj.insert(cell[n - 1][m].room),
                node[cell[n - 1][m].room].adj.insert(cell[n][m].room);
            if (m && cell[n][m - 1].id != cell[n][m].id)
                node[cell[n][m].room].adj.insert(cell[n][m - 1].room),
                node[cell[n][m - 1].room].adj.insert(cell[n][m].room);
        }
    for (int r = 0; r < R; ++r)
        for (int s : node[r].adj)
            if (!seen_edge[{r, s}])
            {
                seen_node.clear();
                double_size = 0;
                double_search(node[r].id, node[s].id, r);
                if (double_size > max_double_size)
                    max_double_size = double_size;
            }
    fout << max_double_size << '\n';
    return 0;
}
```
