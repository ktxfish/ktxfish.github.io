---
title: Painting the Barn | USACO 2019 Feb Silver
date: 2020-08-16 12:30:00
category: USACO
tags:
- prefix sum
- 2D prefix sum
---

<!--more-->

```c++
#include <fstream>

#define MAX_X 1000
#define MAX_Y 1000

std::ifstream fin("paintbarn.in");
std::ofstream fout("paintbarn.out");

int N, K;
int cans[MAX_X + 1][MAX_Y + 1];
int optimal;

int main()
{
    fin >> N >> K;
    for (int n = 0; n < N; ++n)
    {
        int x1, y1, x2, y2;
        fin >> x1 >> y1 >> x2 >> y2;
        ++cans[x1][y1];
        ++cans[x2][y2];
        --cans[x1][y2];
        --cans[x2][y1];
    }
    for (int x = 0; x < MAX_X; ++x)
        for (int y = 0; y < MAX_Y; ++y)
        {
            if (x)
                cans[x][y] += cans[x - 1][y];
            if (y)
                cans[x][y] += cans[x][y - 1];
            if (x && y)
                cans[x][y] -= cans[x - 1][y - 1];
            if (cans[x][y] == K)
                ++optimal;
        }
    fout << optimal << '\n';
    return 0;
}
```
