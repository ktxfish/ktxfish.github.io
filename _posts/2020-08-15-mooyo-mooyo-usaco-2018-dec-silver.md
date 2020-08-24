---
title: Mooyo Mooyo | USACO 2018 Dec Silver
date: 2020-08-15 18:30:00
category: USACO
tags:
- flood fill
---

<!--more-->

```c++
#include <fstream>

#define MAX_N 100

std::ifstream fin("mooyomooyo.in");
std::ofstream fout("mooyomooyo.out");

int N, K;
int board[MAX_N][10], region[MAX_N][10], r, k, v;

void flood_fill(int x, int y)
{
    if (region[x][y])
        return;
    region[x][y] = r + 1;
    ++k;
    if (x - 1 >= 0 && board[x - 1][y] == board[x][y])
        flood_fill(x - 1, y);
    if (y - 1 >= 0 && board[x][y - 1] == board[x][y])
        flood_fill(x, y - 1);
    if (x + 1 < N && board[x + 1][y] == board[x][y])
        flood_fill(x + 1, y);
    if (y + 1 < 10 && board[x][y + 1] == board[x][y])
        flood_fill(x, y + 1);
}

int main()
{
    fin >> N >> K;
    fin.get();
    for (int n = 0; n < N; ++n)
    {
        for (int m = 0; m < 10; ++m)
        {
            char c;
            fin.get(c);
            board[n][m] = c - '0';
        }
        fin.get();
    }
    while (true)
    {
        r = 0, v = 0;
        for (int i = 0; i < N; ++i)
            for (int j = 0; j < 10; ++j)
                region[i][j] = 0;
        for (int n = 0; n < N; ++n)
            for (int m = 0; m < 10; ++m)
                if (board[n][m] && !region[n][m])
                {
                    k = 0;
                    flood_fill(n, m);
                    if (k >= K)
                    {
                        for (int i = 0; i < N; ++i)
                            for (int j = 0; j < 10; ++j)
                                if (region[i][j] == r + 1)
                                    board[i][j] = 0;
                        ++v;
                    }
                    ++r;
                }
        if (!v)
        {
            for (int n = 0; n < N; ++n)
            {
                for (int m = 0; m < 10; ++m)
                    fout << board[n][m];
                fout << '\n';
            }
            return 0;
        }
        for (int m = 0; m < 10; ++m)
            for (int n = N - 2; n >= 0; --n)
                if (board[n][m])
                {
                    int i = n;
                    while (i + 1 < N && !board[i + 1][m])
                    {
                        board[i + 1][m] = board[i][m];
                        board[i][m] = 0;
                        ++i;
                    }
                }
    }    
}
```
