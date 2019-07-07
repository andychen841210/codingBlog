---
title: "AtCoder ABC121"
date: 2019-04-28T14:52:06+08:00
draft: false
lastmod: 2019-04-28T14:52:06+08:00
tags: ["Xor"]
categories: ["Competitive Programming", "AtCoder"]
---
## [AtCoder ABC121](https://atcoder.jp/contests/abc121)

## D

### Solution sketch

XOR 性質題或是規律題

<!--more-->

### 做法 1
```
0000
0001
0010
0011
0100
0101
0110
0111
...
```

會發現最右邊的 bit 是以 2 次為循環，接著的第二個 bit 是以 4 次為循環，以此類推，要注意只有第一個 bit 一次循環是不會歸零的

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

long long int f(long long int x){
    
    if (x <= 0)
        return 0;

    long long int dig[40] = {0};
    for (int i = 1; i < 40; i++){
        long long int mod = 1LL << (i + 1);
        
        if (x % mod - (mod / 2) + 1 > 0)
            dig[i] += (x % mod - (mod / 2) + 1) % 2;
    }

    long long int ans = 0;
    for (int i = 1; i < 40; i++)
        ans |= (dig[i] << i);
    
    if (((x + 1)/ 2) % 2 == 1)
        ans |= 1;

    return ans;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);

    long long int a, b;
    cin >> a >> b;
    
    cout << (f(b) ^ f(a - 1)) << '\n';

    return 0;
}
```

### 作法 2 (官方):

$0 \oplus 1 \oplus 2 \oplus 3 \oplus 4 \oplus 5 ...$ = $(0 \oplus 1) \oplus (2 \oplus 3) \oplus (4 \oplus 5) ...$ = $1 \oplus 1 \oplus 1 ...$

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    long long int a, b;
    cin >> a >> b;

    if (a == b){
        cout << a << '\n';
        return 0;
    }

    if (a % 2 == 0 && b % 2 == 0){
        if (((b - a) / 2) % 2 == 0)
            cout << b << '\n';
        else
            cout << (b ^ 1LL) << '\n';
    }
    else if (a % 2 == 0 && b % 2 == 1){
        if (((b - a + 1) / 2) % 2 == 0)
            cout << 0 << '\n';
        else
            cout << 1 << '\n';
    }
    else if (a % 2 == 1 && b % 2 == 0){
        if (((b - a - 1) / 2) % 2 == 0)
            cout << (a ^ b) << '\n';
        else
            cout << (a ^ b ^ 1LL) << '\n';
    }
    else if (a % 2 == 1 && b % 2 == 1){
        if (((b - a) / 2) % 2 == 0)
            cout << a << '\n';
        else
            cout << (a ^ 1LL) << '\n';
    }

    return 0;
}
```