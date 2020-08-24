---
title: The Bovine Shuffle | USACO 2017 Dec Silver
date: 2020-08-19 21:45:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>
#include <queue>
#include <vector>

#define MAX_N 100000

std::ifstream fin("shuffle.in");
std::ofstream fout("shuffle.out");

int N;
std::vector<int> child[MAX_N];
int parent[MAX_N];
std::queue<int> orphan;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        int c;
        fin >> c;
        --c;
        child[n].push_back(c);
        ++parent[c];
    }
    int always = N;
    for (int n = 0; n < N; ++n)
        if (!parent[n])
            orphan.push(n);
    while (!orphan.empty())
    {
        int n = orphan.front();
        orphan.pop();
        --always;
        for (int c : child[n])
        {
            --parent[c];
            if (!parent[c])
                orphan.push(c);
        }
    }
    fout << always << '\n';
    return 0;
}
```
