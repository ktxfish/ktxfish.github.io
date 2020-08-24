---
title: Convention II | USACO 2018 Dec Silver
date: 2020-08-14 19:00:00
category: USACO
tags:
- priority queue
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>
#include <queue>
#include <utility>

#define MAX_N 100000

std::ifstream fin("convention2.in");
std::ofstream fout("convention2.out");

int N;
std::pair<int, std::pair<int, int>> cows[MAX_N]; // arrival, (seniority, duration)
std::priority_queue<std::pair<int, int>> waiting; // seniority, index
int max_wait;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> cows[n].first >> cows[n].second.second, cows[n].second.first = N - n;
    std::sort(cows, cows + N);

    int now = -1;
    for (int n = 0; n < N; )
    {
        int x = n, m = n + 1;
        while (m < N && cows[m].first < now)
        {
            if (cows[m].second.first < cows[x].second.first)
                x = m;
            ++m;
        }
        if (!waiting.size() && cows[x].first >= now)
            now = cows[x].first + cows[x].second.second;
        else
            waiting.push({cows[x].second.first, x});
        for (int i = n; i < m; ++i)
            if (i != x)
                waiting.push({cows[i].second.first, i});
        while (waiting.size() && (m == N || cows[m].first > now + cows[waiting.top().second].second.second))
        {
            if (now - cows[waiting.top().second].first > max_wait)
                max_wait = now - cows[waiting.top().second].first;
            now += cows[waiting.top().second].second.second;
            waiting.pop();
        }
        n = m;
    }
    fout << max_wait << '\n';
    return 0;
}
```
