---
title: Left Out | USACO 2019 US Open Silver
date: 2020-08-16 18:00:00
category: USACO
tags:
- invariant
---

<!--more-->

```c++
#include <fstream>
#include <string>

#define MAX_N 1000

std::ifstream fin("leftout.in");
std::ofstream fout("leftout.out");

int N;
std::string row[MAX_N];

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> row[n];
    for (int i = N - 1; i >= 0; --i)
    {
        for (int j = i - 1; j >= 0; --j)
            if (row[j] != row[i])
            {
                int x = -1, y = -1;
                bool found = false;
                for (int n = 0; n < N; ++n)
                    if (row[n] != row[i] && row[n] != row[j])
                    {
                        if (found)
                            goto next_pair;
                        else
                        {
                            found = true;
                            y = n;
                            int f = 0;
                            int d = MAX_N;
                            for (int m = 0; m < N; ++m)
                                if (row[n][m] != row[i][m])
                                {
                                    if (d != MAX_N)
                                    {
                                        d = -1;
                                        break;
                                    }
                                    d = m;
                                }
                            if (d != -1)
                                x = d;
                            else
                            {
                                int e = MAX_N;
                                for (int m = 0; m < N; ++m)
                                {
                                    if (row[n][m] != row[j][m])
                                    {
                                        if (e != MAX_N)
                                        {
                                            fout << "-1\n";
                                            return 0;
                                        }
                                        e = m;
                                    }
                                }
                                x = e;
                            }
                        }
                    }
                if (found)
                {
                    fout << y + 1 << ' ' << x + 1 << '\n';
                    return 0;
                }
                else
                {
                    fout << "-1\n";
                    return 0;
                }
            }
        next_pair: ;
    }
    fout << "-1\n";
    return 0;
}
```
