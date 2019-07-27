---
title: "AtCoder ABC127"
date: 2019-07-06T22:35:08+08:00
draft: false
lastmod: 2019-07-06T22:35:08+08:00
tags: ["Math"]
categories: ["Competitive Programming", "AtCoder"]
---
## [AtCoder ABC127](https://atcoder.jp/contests/abc127)

## E

### Solution sketch

x, y 座標對答案貢獻是獨立的，因此可以分開來計算

接著枚舉橫坐標的差值 d，固定兩個橫坐標差值為 d 的點，會有 (n - d) * $ m^2 $ 個，因此差值為 d 的貢獻總共有 (n - d) * $ m^2 $ * d

同理枚舉縱座標，(m - d) * $ n^2 $ * d

最後還要乘上 $ C \binom{n * m - 2}{k - 2} $ 就是答案了，這個式子表示固定兩個點之後，剩下的點中選 k - 2 個，表示這固定兩點總共可以貢獻的次數，由於 n * m 太大且在 mod p 底下，因此使用 lucas 來計算

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

const int MOD = 1e9 + 7;

ll fast_pow(ll a, ll b, ll p){
    ll ans = 1;
    ll base = a % p;
    b = b % (p - 1);
    
    while(b){
        if (b & 1)
            ans = (ans * base) % p;
        
        base = (base * base) % p;
        b >>= 1;
    }

    return ans;
}

ll inv(ll a, ll p){
    return fast_pow(a, p - 2, p);
}

ll C(ll n, ll m, ll p){
    if (n < m)
        return 0;

    m = min(m, n - m);
    
    ll nom = 1, den = 1;
    for (ll i = 1; i <= m; i++){
        nom = (nom * (n - i + 1)) % p;
        den = (den * i) % p;
    }

    return (nom * inv(den, p)) % p;
}

ll lucas(ll n, ll m, ll p){
    if (m == 0)
        return 1;
    return C(n % p, m % p, p) * lucas(n / p, m / p, p) % p;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    ll n, m, k;
    cin >> n >> m >> k;

    ll ans = 0;
    for (ll i = 1; i < n; i++){
        ans += ((((((n - i) * i) % MOD) * m) % MOD) * m) % MOD;
        ans %= MOD;
    }

    for (ll i = 1; i < m; i++){
        ans += ((((((m - i) * i) % MOD) * n) % MOD) * n) % MOD;
        ans %= MOD;
    }
    
    cout << (ans * lucas(n * m - 2, k - 2, MOD)) % MOD << '\n';

    return 0;
}
```

## F

### Solution sketch

|x - a1| + |x - a2| + ... + |x - an| 中算絕對值得最小值，則就是 x 為 a1 到 an 中的中位數，可想成 x 到 a1 ~ an 中的距離。

利用兩個 priority queue 紀錄中位數，然後用 large 紀錄 priority queue 的值比中位數大的總和，small 紀錄 priority queue 的值比中位數小的總和，query 的時候就可以看中位數 x 是誰，然後用 large - x * large_priority_queue_size + x * small_priority_queue_size - small，這行算式就表示整個絕對值的總和，因為在大的 priority queue 中的值一定會大於等於中位數，而在小的 priority queue 中的值一定是小於等於中位數，因此這樣可以快速的計算絕對值的總和，而不用每次 query 的時候整個掃一遍，最後再加上 sum_b 的總和就是答案了

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long int ll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    int Q;
    cin >> Q;
    
    priority_queue<ll> mx; // put the small
    priority_queue<ll, vector<ll>, greater<ll>> mn; // put the large
    ll sum_b = 0, cnt = 0, large = 0, small = 0;
    for (int q = 0; q < Q; q++){
        int a;
        cin >> a;

        if (a == 1){
            ll b, c;
            cin >> b >> c;
            mn.push(b);
            large += b;
            while((int) mn.size() - (int) mx.size() > 1){
                mx.push(mn.top());
                large -= mn.top();
                small += mn.top();
                mn.pop();
            }

            while(!mx.empty() && mn.top() < mx.top()){
                ll tmp1 = mn.top(), tmp2 = mx.top();
                mn.pop();
                mx.pop();
                mn.push(tmp2);
                mx.push(tmp1);
                large += tmp2 - tmp1;
                small += tmp1 - tmp2;
            }
            sum_b += c;
            cnt += 1;
        }
        else{
            ll x;
            if (cnt % 2 == 0)
                x = mx.top();
            else
                x = mn.top();

            cout << x << " " << large - x * (ll) mn.size() + x * (ll) mx.size() - small + sum_b << '\n';
        }
    }

    return 0;
}
```