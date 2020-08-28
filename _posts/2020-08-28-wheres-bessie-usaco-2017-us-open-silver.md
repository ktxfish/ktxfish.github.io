---
title: Where’s Bessie? | USACO 2017 US Open Silver
date: 2020-08-28 12:00:00
category: USACO
tags:
- flood fill
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 20

std::ifstream fin("where.in");
std::ofstream fout("where.out");

int N, C;
char color[MAX_N][MAX_N];
int comp[MAX_N][MAX_N];
bool subset[MAX_N][MAX_N][MAX_N][MAX_N];
int pcl;

void flood_fill(int n, int m, int x, int y, int s, int t)
{
    if (comp[n][m] != -1)
        return;
    comp[n][m] = C;
    if (n > x && color[n - 1][m] == color[n][m])
        flood_fill(n - 1, m, x, y, s, t);
    if (m > y && color[n][m - 1] == color[n][m])
        flood_fill(n, m - 1, x, y, s, t);
    if (n < x + s && color[n + 1][m] == color[n][m])
        flood_fill(n + 1, m, x, y, s, t);
    if (m < y + t && color[n][m + 1] == color[n][m])
        flood_fill(n, m + 1, x, y, s, t);
}

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        fin.get();
        for (int m = 0; m < N; ++m)
            fin.get(color[n][m]), color[n][m] -= 'A';
    }
    for (int s = N - 1; s >= 0; --s)
        for (int t = N - 1; t >= 0; --t)
            for (int x = 0; x < N - s; ++x)
                for (int y = 0; y < N - t; ++y)
                    if (!subset[x][y][s][t])
                    {
                        C = 0;
                        for (int n = x; n <= x + s; ++n)
                            for (int m = y; m <= y + t; ++m)
                                comp[n][m] = -1;
                        for (int n = x; n <= x + s; ++n)
                            for (int m = y; m <= y + t; ++m)
                                if (comp[n][m] == -1)
                                    flood_fill(n, m, x, y, s, t), ++C;

                        bool seen_color[26] = {};
                        int num_color = 0;
                        for (int n = x; n <= x + s; ++n)
                            for (int m = y; m <= y + t; ++m)
                                if (!seen_color[color[n][m]])
                                    ++num_color, seen_color[color[n][m]] = true;
                        if (num_color != 2)
                            continue;

                        bool seen_comp[MAX_N * MAX_N] = {};
                        int num_comp[2] = {};
                        char first = color[x][y];
                        for (int n = x; n <= x + s; ++n)
                            for (int m = y; m <= y + t; ++m)
                                if (color[n][m] == first && !seen_comp[comp[n][m]])
                                    ++num_comp[0], seen_comp[comp[n][m]] = true;
                                else if (color[n][m] != first && !seen_comp[comp[n][m]])
                                    ++num_comp[1], seen_comp[comp[n][m]] = true;

                        if ((num_comp[0] == 1 && num_comp[1] >= 2) || (num_comp[1] == 1 && num_comp[0] >= 2))
                        {
                            ++pcl;
                            for (int p = s; p >= 0; --p)
                                for (int q = t; q >= 0; --q)
                                    for (int i = x; i <= x + s - p; ++i)
                                        for (int j = y; j <= y + t - q; ++j)
                                            subset[i][j][p][q] = true;
                        }
                }
    fout << pcl << '\n';
    return 0;
}
```
