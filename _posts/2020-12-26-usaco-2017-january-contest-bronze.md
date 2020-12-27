---
title: USACO 2017 January Contest, Bronze
date: 2020-12-26 19:30:00
category: USACO
tags:
---

<!--more-->

# Don't Be Last!

```c++
#include <fstream>
#include <map>
#include <string>

#define MAX_N 100
#define MAX_AMT 100
#define NAMES {"Bessie", "Elsie", "Daisy", "Gertie", "Annabelle", "Maggie", "Henrietta"}

std::ifstream fin("notlast.in");
std::ofstream fout("notlast.out");

int N;
std::map<std::string, int> milk;

int main() {
    for (std::string name : NAMES)
        milk[name] = 0;
    fin >> N;
    for (int n = 0; n < N; ++n) {
        std::string name;
        int amt;
        fin >> name >> amt;
        milk[name] += amt;
    }
    int min_amt = MAX_N * MAX_AMT;
    int min2_amt = MAX_N * MAX_AMT + 1;
    for (std::pair<std::string, int> cow : milk)
        if (cow.second < min_amt) {
            int t = min_amt;
            min_amt = cow.second;
            min2_amt = t;
        } else if (cow.second > min_amt && cow.second < min2_amt)
            min2_amt = cow.second;

    bool multi = false;
    std::string min2_name = "";
    for (std::pair<std::string, int> cow : milk)
        if (cow.second == min2_amt) {
            if (min2_name == "")
                min2_name = cow.first;
            else
                multi = true;
        }
    fout << (multi || min2_name == "" ? "Tie\n" : min2_name + '\n');
    return 0;
}
```

# Hoof, Paper, Scissors

```c++
#include <fstream>

#define MAX_N 100

std::ifstream fin("hps.in");
std::ofstream fout("hps.out");

int N;
int rec[MAX_N][2];
const int rot[6][3] = {
    {1, 2, 3},
    {1, 3, 2},
    {2, 1, 3},
    {2, 3, 1},
    {3, 1, 2},
    {3, 2, 1}
    };
int max_win;

bool winning(const int (& game)[2]) {
    // assume 1 = hoof, 2 = paper, 3 = scissors
    if (game[0] == 1) {
        if (game[1] == 3)
            return true;
        return false;
    } else if (game[0] == 2) {
        if (game[1] == 1)
            return true;
        return false;
    } else if (game[1] == 2)
        return true;
    return false;
}

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n)
        fin >> rec[n][0] >> rec[n][1];
    for (int r = 0; r < 6; ++r) {
        int win = 0;
        for (int n = 0; n < N; ++n)
            if (winning({
                rot[r][rec[n][0] - 1],
                rot[r][rec[n][1] - 1]
                }))
                ++win;
        if (win > max_win)
            max_win = win;
    }
    fout << max_win << '\n';
    return 0;
}
```

# Cow Tipping

```c++
#include <fstream>

#define MAX_N 10

std::ifstream fin("cowtip.in");
std::ofstream fout("cowtip.out");

int N;
bool tip[MAX_N][MAX_N];
int app;

int main() {
    fin >> N;
    for (int n = 0; n < N; ++n) {
        fin.get();
        for (int m = 0; m < N; ++m) {
            char c;
            fin.get(c);
            if (c == '1')
                tip[n][m] = true;
        }
    }
    for (int i = N - 1; i >= 0; --i)
        for (int j = N - 1; j >= 0; --j)
            if (tip[i][j]) {
                for (int n = 0; n <= i; ++n)
                    for (int m = 0; m <= j; ++m)
                        tip[n][m] = !tip[n][m];
                ++app;
            }
    fout << app << '\n';
    return 0;
}
```
