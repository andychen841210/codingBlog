---
title: "Contest123"
date: 2019-04-17T10:34:10+08:00
draft: false
lastmod: 2019-04-17T10:34:10+08:00
tags: ["Bfs"]
categories: ["Competitive Programming", "AtCoder"]
---
## [Contest123](https://atcoder.jp/contests/abc123)

## D

### Solution sketch

做 Bfs 即可，一開始以最大的 sum 作為起點，接著下次的選擇則為 x 換為次大或 y 換為次大或 z 換為次大即可

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Node{
    long long int sum;
    int x, y, z;

    bool operator < (const Node &node) const{
        return sum < node.sum;
    }
};

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    int x, y, z, k;
    cin >> x >> y >> z >> k;

    long long int a[2000], b[2000], c[2000];
    for (int i = 0; i < x; i++)
        cin >> a[i];

    for (int i = 0; i < y; i++)
        cin >> b[i];

    for (int i = 0; i < z; i++)
        cin >> c[i];

    sort(a, a + x);
    sort(b, b + y);
    sort(c, c + z);
    
    priority_queue<Node> pq;
    set<pair<int, pair<int,int>> > s;
    pq.push({a[x - 1] + b[y - 1] + c[z - 1], x - 1, y - 1, z - 1});
    s.insert({x - 1, {y - 1, z - 1}});
    
    for (int i = 0; i < k; i++){
        Node n = pq.top(); pq.pop();
        cout << n.sum << '\n';

        if (n.x > 0){
            if (s.find({n.x - 1, {n.y, n.z}}) == s.end()){
                pq.push({a[n.x - 1] + b[n.y] + c[n.z], n.x - 1, n.y, n.z});
                s.insert({n.x - 1, {n.y, n.z}});
            }
        }
        if (n.y > 0){
            if (s.find({n.x, {n.y - 1, n.z}}) == s.end()){
                pq.push({a[n.x] + b[n.y - 1] + c[n.z], n.x, n.y - 1, n.z});
                s.insert({n.x, {n.y - 1, n.z}});
            }
        }
        if (n.z > 0){
            if (s.find({n.x, {n.y, n.z - 1}}) == s.end()){
                pq.push({a[n.x] + b[n.y] + c[n.z - 1], n.x, n.y, n.z - 1});
                s.insert({n.x, {n.y, n.z - 1}});
            }
        }
    }

    return 0;
}
```