---
title: Lifeguards | USACO 2018 Jan Silver
date: 2020-08-20 11:40:00
category: USACO
tags:
- linear scan
---

<!--more-->

```c++
#include <algorithm>
#include <fstream>

#define MAX_N 100000

std::ifstream fin("lifeguards.in");
std::ofstream fout("lifeguards.out");

struct Shift
{
    int beg, end;
};

int N;
Shift shifts[MAX_N];
int ttl, nxt, min_cov;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> shifts[n].beg >> shifts[n].end;
    std::sort(shifts, shifts + N, [](const Shift &a, const Shift &b) { return a.beg < b.beg; });
    nxt = shifts[0].end, ttl += nxt - shifts[0].beg;
    min_cov = (1 < N ? std::min(shifts[1].beg, shifts[0].end) : shifts[0].end) - shifts[0].beg;
    for (int n = 1; n < N; ++n)
    {
        int cov = (n + 1 < N ? std::min(shifts[n + 1].beg, shifts[n].end) : shifts[n].end) -
                  std::max(nxt, shifts[n].beg);
        min_cov = std::min(min_cov, (cov > 0 ? cov : 0));
        if (shifts[n].beg > nxt)
            nxt = shifts[n].end, ttl += nxt - shifts[n].beg;
        else if (shifts[n].end > nxt)
            ttl += shifts[n].end - nxt, nxt = shifts[n].end;
    }
    fout << ttl - min_cov << '\n';
    return 0;
}
```
