---
title: Secret Cow Code | USACO 2017 Jan Silver
date: 2020-08-25 14:30:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>
#include <string>

std::ifstream fin("cowcode.in");
std::ofstream fout("cowcode.out");

long long N;
std::string s;

int main()
{
    fin >> s >> N;
    int l = s.length();
    long long p = l;
    while (p < N)
        p *= 2;
    while (N > l)
    {
        while (p >= N)
            p /= 2;
        if (N == p + 1)
            --N;
        else
            N -= p + 1;
    }
    fout << s[N - 1] << '\n';
    return 0;
}
```
