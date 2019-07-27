---
title: "Round560F"
date: 2019-07-27T19:51:12+08:00
draft: false
lastmod: 2019-07-27T19:51:12+08:00
tags: ["Binary Search", "Greedy"]
categories: ["Competitive Programming", "Codeforces", "Division 3"]
---
## [Codeforces Round560 Division 3](https://codeforces.com/contest/1165)

## F

### Solution sketch

binary search 搜答案需要幾天，然後 check function 需要檢查這個天數內是否可行，
由於同一種物品可能在很多天進行打折，那麼要在哪天購買才是最好的呢？

由於一天可以購買的數量沒有限制，那其實只要在那種物品有打折的最後一天一次進行購買即可，
因此只要分別紀錄每個物品最接近這次搜尋答案的日子，接著檢查這次搜尋的答案是否可行即可

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m;
int k[200100];
vector<int> v[200100];

bool C(int mid){
    vector<int> last[mid + 2];
    vector<int> rem(200100);
    int total = 0;
    for (int i = 1; i <= n; i++){
        rem[i] = k[i];
        total += rem[i];
        if ((int) v[i].size() == 0 || v[i][0] > mid)
            continue;
        int x = *(upper_bound(v[i].begin(), v[i].end(), mid) - 1);
        last[x].push_back(i);
    }

    int cost = 0;
    for (int i = 1; i <= mid; i++){
        cost += 1;
        for (auto j: last[i]){
            while(rem[j] && cost){
                cost--;
                rem[j]--;
                total--;
            }
        }
    }

    if (total * 2 > cost)
        return false;
    return true;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >> n >> m;

    for (int i = 1; i <= n; i++){
        cin >> k[i];
    }

    for (int i = 0; i < m; i++){
        int d, t;
        cin >> d >> t;
        v[t].push_back(d);
    }

    for (int i = 1; i <= n; i++)
        sort(v[i].begin(), v[i].end());

    int lb = 0, ub = 4e5 + 10;
    while(ub - lb > 1){
        int mid = (lb + ub) / 2;
        if (C(mid))
            ub = mid;
        else
            lb = mid;
    }

    cout << ub << '\n';

    return 0;
}
```
