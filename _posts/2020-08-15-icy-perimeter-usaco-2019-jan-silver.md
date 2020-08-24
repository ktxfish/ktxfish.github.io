---
title: Icy Perimeter | USACO 2019 Jan Silver
date: 2020-08-15 22:00:00
category: USACO
tags:
- flood fill
- vector
- pair
---

<!--more-->

```c++
#include <fstream>
#include <vector>
#include <utility>

#define MAX_N 1000

std::ifstream fin("perimeter.in");
std::ofstream fout("perimeter.out");

int N;
char grid[MAX_N][MAX_N];
bool blobed[MAX_N][MAX_N];
int min_perim = MAX_N * MAX_N * 4, max_size;
std::vector<std::pair<int, int>> cur_blob;
std::vector<std::vector<std::pair<int, int>>> max_blobs;

void flood_fill(int x, int y)
{
    if (blobed[x][y])
        return;
    blobed[x][y] = true;
    cur_blob.push_back({x, y});

    if (x && grid[x - 1][y] == '#')
        flood_fill(x - 1, y);
    if (y && grid[x][y - 1] == '#')
        flood_fill(x, y - 1);
    if (x + 1 < N && grid[x + 1][y] == '#')
        flood_fill(x + 1, y);
    if (y + 1 < N && grid[x][y + 1] == '#')
        flood_fill(x, y + 1);
}

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        fin.get();
        for (int m = 0; m < N; ++m)
            fin.get(grid[n][m]);
    }
    for (int n = 0; n < N; ++n)
        for (int m = 0; m < N; ++m)
            if (grid[n][m] == '#' && !blobed[n][m])
            {
                cur_blob.clear();
                flood_fill(n, m);
                if (cur_blob.size() == max_size)
                    max_blobs.push_back(cur_blob);
                else if (cur_blob.size() > max_size)
                {
                    max_size = cur_blob.size();
                    max_blobs.clear();
                    max_blobs.push_back(cur_blob);
                }
            }
    for (std::vector<std::pair<int, int>> max_blob : max_blobs)
    {
        int perim = 0;
        for (std::pair<int, int> loc : max_blob)
        {
            if (!loc.first || grid[loc.first - 1][loc.second] != '#')
                ++perim;
            if (!loc.second || grid[loc.first][loc.second - 1] != '#')
                ++perim;
            if (loc.first + 1 == N || grid[loc.first + 1][loc.second] != '#')
                ++perim;
            if (loc.second + 1 == N || grid[loc.first][loc.second + 1] != '#')
                ++perim;
        }
        if (perim < min_perim)
            min_perim = perim;
    }
    fout << max_size << ' ' << min_perim << '\n';
    return 0;
}
```
