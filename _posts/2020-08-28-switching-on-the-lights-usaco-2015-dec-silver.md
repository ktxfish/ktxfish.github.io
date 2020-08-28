---
title: Switching on the Lights | USACO 2015 Dec Silver
date: 2020-08-28 17:45:00
category: USACO
tags:
- depth first search
---

<!--more-->

```c++
#include <fstream>
#include <utility>
#include <vector>

#define MAX_N 100

std::ifstream fin("lightson.in");
std::ofstream fout("lightson.out");

struct Room
{
    bool seen = false;
    bool lit = false;
    std::vector<std::pair<int, int>> toggle;
};

int N, M;
Room room[MAX_N][MAX_N];
int illuminate = 1;

void search(int x, int y)
{
    if (room[x][y].seen)
        return;
    room[x][y].seen = true;
    for (std::pair<int, int> tog : room[x][y].toggle)
    {
        int a = tog.first, b = tog.second;
        if (room[a][b].lit)
            continue;
        room[a][b].lit = true;
        ++illuminate;
        if ((a && room[a - 1][b].seen) || (b && room[a][b - 1].seen) ||
            (a + 1 < N && room[a + 1][b].seen) || (b + 1 < N && room[a][b + 1].seen))
            search(a, b);
    }
    if (x && room[x - 1][y].lit)
        search(x - 1, y);
    if (y && room[x][y - 1].lit)
        search(x, y - 1);
    if (x + 1 < N && room[x + 1][y].lit)
        search(x + 1, y);
    if (y + 1 < N && room[x][y + 1].lit)
        search(x, y + 1);
}

int main()
{
    fin >> N >> M;
    for (int m = 0; m < M; ++m)
    {
        int x, y, a, b;
        fin >> x >> y >> a >> b;
        room[x - 1][y - 1].toggle.push_back({a - 1, b - 1});
    }
    room[0][0].lit = true;
    search(0, 0);
    fout << illuminate << '\n';
    return 0;
}
```
