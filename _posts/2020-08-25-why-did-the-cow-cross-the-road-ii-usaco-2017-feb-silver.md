---
title: Why Did the Cow Cross the Road II | USACO 2017 Feb Silver
date: 2020-08-25 21:50:00
category: USACO
tags:
- flood fill
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100
#define MAX_K 100

std::ifstream fin("countcross.in");
std::ofstream fout("countcross.out");

struct Field
{
    bool north = true, south = true, east = true, west = true;
    int comp = -1;
};

int N, K, R, P;
Field field[MAX_N][MAX_N];
int cow_r[MAX_K], cow_c[MAX_K];
int distant;

void flood_fill(int r, int c)
{
    field[r][c].comp = P;
    if (field[r][c].north && field[r - 1][c].comp == -1)
        flood_fill(r - 1, c);
    if (field[r][c].south && field[r + 1][c].comp == -1)
        flood_fill(r + 1, c);
    if (field[r][c].west && field[r][c - 1].comp == -1)
        flood_fill(r, c - 1);
    if (field[r][c].east && field[r][c + 1].comp == -1)
        flood_fill(r, c + 1);
}

int main()
{
    fin >> N >> K >> R;
    for (int i = 0; i < R; ++i)
    {
        int r1, c1, r2, c2;
        fin >> r1 >> c1 >> r2 >> c2;
        --r1, --c1, --r2, --c2;
        if (r1 == r2 && c1 < c2)
            field[r1][c1].east = false, field[r2][c2].west = false;
        else if (r1 == r2)
            field[r1][c1].west = false, field[r2][c2].east = false;
        else if (r1 < r2)
            field[r1][c1].south = false, field[r2][c2].north = false;
        else
            field[r1][c1].north = false, field[r2][c2].south = false;
    }
    for (int c = 0; c < N; ++c)
        field[0][c].north = false, field[N - 1][c].south = false;
    for (int r = 0; r < N; ++r)
        field[r][0].west = false, field[r][N - 1].east = false;

    for (int r = 0; r < N; ++r)
        for (int c = 0; c < N; ++c)
            if (field[r][c].comp == -1)
                flood_fill(r, c), ++P;

    for (int k = 0; k < K; ++k)
        fin >> cow_r[k] >> cow_c[k], --cow_r[k], --cow_c[k];
    for (int i = 0; i < K; ++i)
        for (int j = i + 1; j < K; ++j)
            if (field[cow_r[i]][cow_c[i]].comp != field[cow_r[j]][cow_c[j]].comp)
                ++distant;
    fout << distant << '\n';
    return 0;
}
```
