---
title: USACO 2017 February Contest, Bronze
date: 2020-12-26 20:40:00
category: USACO
tags:
---

<!--more-->

# Why Did the Cow Cross the Road

```c++
#include <fstream>

#define MAX_ID 10

std::ifstream fin("crossroad.in");
std::ofstream fout("crossroad.out");

int N;
int side[MAX_ID + 1];
int cross;

int main() {
    for (int id = 1; id <= 10; ++id)
        side[id] = -1;
    fin >> N;
    for (int n = 0; n < N; ++n) {
        int id, s;
        fin >> id >> s;
        if (side[id] == -1)
            side[id] = s;
        else if (side[id] != s)
            ++cross, side[id] = s;
    }
    fout << cross << '\n';
    return 0;
}
```

# Why Did the Cow Cross the Road II

```c++
#include <algorithm>
#include <fstream>

std::ifstream fin("circlecross.in");
std::ofstream fout("circlecross.out");

struct Cow {
    int a = -1, b;
} cow[26];
int pair;

int main() {
    for (int p = 0; p < 52; ++p) {
        char c;
        fin.get(c);
        int i = c - 'A';
        if (cow[i].a == -1)
            cow[i].a = p;
        else
            cow[i].b = p;
    }
    std::sort(cow, cow + 26, [](Cow a, Cow b){ return a.a < b.a; });
    for (int i = 0; i < 26; ++i)
        for (int j = i + 1; j < 26; ++j)
            if (cow[j].a < cow[i].b && cow[i].b < cow[j].b)
                ++pair;
    fout << pair << '\n';
    return 0;
}
```

# Why Did the Cow Cross the Road III

```c++
#include <fstream>
#include <functional>
#include <queue>
#include <utility>
#include <vector>

std::ifstream fin("cowqueue.in");
std::ofstream fout("cowqueue.out");

int N;
std::priority_queue<
    std::pair<int, int>,
    std::vector<std::pair<int, int>>,
    std::greater<std::pair<int, int>>
    > line;
int now;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n) {
        int a, b;
        fin >> a >> b;
        line.push({a, b});
    }
    while (!line.empty()) {
        std::pair<int, int> cow = line.top();
        line.pop();
        if (now < cow.first)
            now = cow.first;
        now += cow.second;
    }
    fout << now << '\n';
    return 0;
}
```
