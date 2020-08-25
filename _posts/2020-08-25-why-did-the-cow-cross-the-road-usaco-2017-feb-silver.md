---
title: Why Did the Cow Cross the Road | USACO 2017 Feb Silver
date: 2020-08-25 17:10:00
category: USACO
tags:
- greedy
- priority queue
- pair
---

<!--more-->

```c++
#include <fstream>
#include <functional>
#include <queue>
#include <utility>

std::ifstream fin("helpcross.in");
std::ofstream fout("helpcross.out");

int C, N, ans;
std::priority_queue<int, std::vector<int>, std::greater<int>> T, leave;
std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>,
                    std::greater<std::pair<int, int>>> intvl;

int main()
{
    fin >> C >> N;
    for (int c = 0; c < C; ++c)
    {
        int t;
        fin >> t;
        T.push(t);
    }
    for (int n = 0; n < N; ++n)
    {
        int a, b;
        fin >> a >> b;
        intvl.push({a, b});
    }
    while (!T.empty())
    {
        int t = T.top();
        T.pop();
        while (!leave.empty() && leave.top() < t)
            leave.pop();
        while (!intvl.empty() && intvl.top().first <= t)
        {
            if (intvl.top().second >= t)
                leave.push(intvl.top().second);
            intvl.pop();
        }
        if (!leave.empty())
            ++ans, leave.pop();
    }
    fout << ans << '\n';
    return 0;
}
```
