---
title: "Edu60DE"
date: 2019-08-27T22:43:23+08:00
draft: false
lastmod: 2019-08-27T22:43:23+08:00
tags: ["Dp", "Matrices", "Fast Pow"]
categories: ["Competitive Programming", "Codeforces", "Educational"]
---
## [Codeforces Educational 60](https://codeforces.com/contest/1117)

## D

### Abridge problem statement

給定一堆個魔法寶石一字排開，每個魔法寶石可以變成 m 個普通的寶石，問說構造出長度為 n 的寶石有幾種方法

<!--more-->

### Solution sketch

如果用遞推的方法，則轉移方程式可以寫成

$$\begin{aligned}
    dp[n] = dp[n - 1] + dp[n - m]
\end{aligned}$$

表示為長度為 n 的寶石排列數，可以由 n - 1 寶石的排列中添加一個普通的寶石，以及在 n - m 個寶石中，添加一個魔法寶石
可以將上面的式子寫成下面的形式

$$
\begin{bmatrix}
    dp\_n \\\ 
    dp\_{n-1} \\\  
    \vdots \\\ 
    dp\_{n-m+1}
\end{bmatrix} =
\begin{bmatrix}
    1 & 0 & 0 & \cdots & 0 & 1 \\\ 
    1 & 0 & 0 & \cdots & 0 & 0 \\\ 
    \vdots & \vdots & \vdots & \cdots & \vdots & \vdots \\\ 
    0 & 0 & 0 & \cdots & 1 & 0
\end{bmatrix} 
\begin{bmatrix}
    dp\_{n-1} \\\ 
    dp\_{n-2} \\\  
    \vdots \\\ 
    dp\_{n-m}
\end{bmatrix}
$$

那麼計算 $dp_n$ 則利用矩陣快速冪的方法來計算即可

$$
\begin{bmatrix}
    dp\_n \\\ 
    dp\_{n-1} \\\  
    \vdots \\\ 
    dp\_{n-m+1}
\end{bmatrix} =
\begin{bmatrix}
    1 & 0 & 0 & \cdots & 0 & 1 \\\ 
    1 & 0 & 0 & \cdots & 0 & 0 \\\ 
    \vdots & \vdots & \vdots & \cdots & \vdots & \vdots \\\ 
    0 & 0 & 0 & \cdots & 1 & 0
\end{bmatrix}^{n-m+1} 
\begin{bmatrix}
    dp\_{m-1} \\\ 
    dp\_{m-2} \\\  
    \vdots \\\ 
    dp\_0
\end{bmatrix}
$$

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef vector<long long> vec;
typedef vector<vec> mat;

const int MOD = 1e9 + 7;

mat mul(mat &A, mat &B){
    int n = A.size(), m = B[0].size(), mm = B.size();
    mat res(n, vec(m));
    for (int i = 0; i < n; i++){
        for (int j = 0; j < m; j++){
            for (int k = 0; k < mm; k++){
                res[i][j] = (res[i][j] + (A[i][k] * B[k][j]) % MOD) % MOD;
            }
        }
    }

    return res;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    long long n, m;
    cin >> n >> m;

    mat B(m, vec(m));
    B[0][0] = B[0][m - 1] = 1;
    for (int i = 1; i < m; i++)
        B[i][i - 1] = 1;
    
    mat A(m, vec(m));
    for (int i = 0; i < m; i++)
        A[i][i] = 1;
    
    if (n < m){
        cout << 1 << '\n';
        return 0;
    }

    long long x = n - m + 1;
    while(x){
        if (x & 1)
            A = mul(A, B);
        B = mul(B, B);
        x >>= 1;
    }
    
    long long ans = 0;
    for (int i = 0; i < m; i++)
        ans = (ans + A[0][i]) % MOD;

    cout << ans << '\n';

    return 0;
}
```

## E

### Abridge problem statement

給定一個長度為 n 字串，這個字串經過了幾次交換而成的，那現在需要你做出最多三次詢問，將字串還原成原本的字串，
而你在詢問的時候是給定一個長度為 n 的字串，他會用他的交換的順序，把你的字串照這個順序做一次並回傳給你

### Solution sketch

詢問三次分別為 

1. 26 * 26 個 a + 26 * 26 個 b + ... + 26 * 26 個 z

2. 26 個 a + 26 個 b + ... + 26 個 z

3. abc...zabc...z...abc...z

第一次詢問完就可以知道個別的字母來自哪個區間，第二次跟第三次同理，只是把範圍在縮得更小，
可以把它想成是位置來源 $x = a * 26^2 + b * 26 + c$

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    string s;
    cin >> s;

    string ss[3];
    int len = s.length();
    vector<int> origin_idx(len);

    for (int i = 0; i < 3; i++){
        for (int j = 0; j < len; j++){
            if (i == 0)
                ss[i] += 'a' + j % 26;
            else if (i == 1)
                ss[i] += 'a' + (j / 26) % 26;
            else if (i == 2)
                ss[i] += 'a' + (j / 26 / 26) % 26;
        }

        cout << "? " << ss[i] << endl;

        string inp;
        cin >> inp;
        for (int j = 0; j < len; j++){
            if (i == 0)
                origin_idx[j] += inp[j] - 'a';
            else if (i == 1)
                origin_idx[j] += (inp[j] - 'a') * 26;
            else if (i == 2)
                origin_idx[j] += (inp[j] - 'a') * 26 * 26;
        }
    }

    string ans(len, ' ');
    for (int i = 0; i < len; i++)
        ans[origin_idx[i]] = s[i];

    cout << "! " << ans << endl;

    return 0;
}
```


