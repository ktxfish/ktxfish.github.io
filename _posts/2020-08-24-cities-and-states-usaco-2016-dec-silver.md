---
title: Cities and States | USACO 2016 Dec Silver
date: 2020-08-24 13:00:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>
#include <map>
#include <string>

std::ifstream fin("citystate.in");
std::ofstream fout("citystate.out");

int N;
std::map<std::string, int> count;
int pairs;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
    {
        std::string city, state;
        fin >> city >> state;
        ++count[{city[0], city[1], state[0], state[1]}];
    }
    for (auto p : count)
    {
        std::string code = {p.first[0], p.first[1], p.first[2], p.first[3]};
        std::string anti = {code[2], code[3], code[0], code[1]};
        if (anti != code && count.count(anti))
            pairs += p.second * count[anti];
    }
    fout << pairs / 2 << '\n';
    return 0;
}
```
