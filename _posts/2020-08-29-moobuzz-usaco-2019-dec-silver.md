---
title: MooBuzz | USACO 2019 Dec Silver
date: 2020-08-29 11:45:00
category: USACO
tags:
---

<!--more-->

```c++
#include <fstream>

std::ifstream fin("moobuzz.in");
std::ofstream fout("moobuzz.out");

int N, m;

int main()
{
    fin >> N;
    switch (N % 8)
    {
    case 0:
        m = -1;
        break;
    case 1:
        m = 0;
        break;
    case 2:
        m = 0;
        break;
    case 3:
        m = 1;
        break;
    case 4:
        m = 3;
        break;
    case 5:
        m = 3;
        break;
    case 6:
        m = 5;
        break;
    case 7:
        m = 6;
        break;
    }
    fout << N + N / 8 * 7 + m << '\n';
    return 0;
}
```
