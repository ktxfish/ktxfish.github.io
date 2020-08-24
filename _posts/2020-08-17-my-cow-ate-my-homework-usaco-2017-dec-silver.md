---
title: My Cow Ate My Homework | USACO 2017 Dec Silver
date: 2020-08-17 21:30:00
category: USACO
tags:
- prefix sum
---

<!--more-->

```c++
#include <fstream>
#include <vector>

#define MAX_N 100000

std::ifstream fin("homework.in");
std::ofstream fout("homework.out");

int N;
int score[MAX_N];
std::vector<int> ans;

int main()
{
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> score[n];
    double max_avg = 0;
    for (int n = N - 2, min = score[N - 1], sum = 0; n > 0; --n)
    {
        if (score[n] < min)
            sum += min, min = score[n];
        else
            sum += score[n];
        double avg = static_cast<double>(sum) / (N - n - 1);
        if (avg + 0.0001 > max_avg && avg - 0.0001 < max_avg)
            ans.push_back(n);
        else if (avg > max_avg)
            max_avg = avg, ans.clear(), ans.push_back(n);
    }
    for (int i = ans.size() - 1; i >= 0; --i)
        fout << ans[i] << '\n';
    return 0;
}
```
