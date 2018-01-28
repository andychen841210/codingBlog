---
title: "Round422ABCD"
date: 2018-01-28T21:27:16+08:00
draft: false
lastmod: 2018-01-28T21:27:16+08:00
tags: ["Math", "String", "Dp"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round422 Division 2](http://codeforces.com/contest/822)

## A

### Abridge problem statement

給定 A 跟 B，問 A! 跟 B! 的最大公因數，min(A, B) <= 12

### Solution sketch

取 min(A, B) 直接坐階乘

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	long long int a, b, ans = 1;
	
	scanf(" %lld %lld", &a, &b);
	
	for (long long int i = 1; i <= min(a, b); i++)
		ans *= i;
	
	printf("%lld\n", ans);
	
	return 0;
}
```

## B

### Abridge problem statement

給定兩個字串 s 跟 t，問說 s 最少換幾個字元，可以變成 t 的 substring

### Solution sketch

直接去模擬比對，看最少的是誰

<!-- more -->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, m;
	char s[10000], t[10000];
	vector<int> ans;

	scanf(" %d %d", &n, &m);
	scanf(" %s", s);
	scanf(" %s", t);
	
	ans.clear();
	int cnt = 1000000;
	for (int i = 0; i < m - n + 1; i++){
			int temp = 0;
			for (int j = 0; j < n; j++)
				if (t[i + j] != s[j])
					temp++;
			if (temp < cnt){
				cnt = temp;
				ans.clear();
				for (int j = 0; j < n; j++)
					if (t[i + j] != s[j])
						ans.push_back(j + 1);
			}
	}

	printf("%d\n", ans.size());
	for (int i = 0; i < ans.size(); i++){
		if (i != 0)
			printf(" %d", ans[i]);
		else
			printf("%d", ans[i]);
	}
	printf("\n");

	return 0;
}
```

## C

### Abridge problem statement

給定 n 個區間，每個區間長度為 l - r + 1，每個區間有一個 cost 值，在恰好滿足兩個不相交區間
的長度等於給定長度 x 的情況下，求出兩個區間最小的 cost 和，若找不到滿足條件的區間則輸出 -1

### Solution sketch

分別對區間的起點和終點分別排序，然後固定起點，在終點數組中找滿足不相交的區間，然後更新距離數組(初始化為無窮大)，
然後判斷是否有滿足條件的距離數組加上當前這個區間的 cost，是否更小，若更小則更新答案

<!-- more -->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Data{
	int l, r;
	long long int cost;
};

bool cmp(Data a, Data b){
	return a.l < b.l;
}

bool cmp1(Data a, Data b){
	return a.r < b.r;
}

Data a[300000], b[300000];
long long int mincost[300000];
int INF = INT_MAX;

int main(){
	int n, x;

	scanf(" %d %d", &n, &x);

	for (int i = 0; i < n; i++){
		int l, r, cost;
		scanf(" %d %d %d", &l, &r, &cost);
		a[i].l = b[i].l = l;
		a[i].r = b[i].r = r;
		a[i].cost = b[i].cost = cost;
	}

	for (int i = 0; i < 300000; i++)
		mincost[i] = INF;
	
	sort(a, a + n, cmp);
	sort(b, b + n, cmp1);
	
	long long int ans = INF, j = 0;
	for (int i = 0; i < n; i++){
		while (j < n && b[j].r < a[i].l){
			mincost[b[j].r - b[j].l + 1] = min(b[j].cost, mincost[b[j].r - b[j].l + 1]);
			j++;
		}
		int k = x - a[i].r + a[i].l - 1;
		if (k > 0)
			ans = min(ans, mincost[k] + a[i].cost);
	}

	if (ans == INF)
		printf("-1\n");
	else
		printf("%lld\n", ans);

	return 0;
}
```


## D

### Abridge problem statement

一場選美比賽有 N 個人，可分成 N / x，每組 x 人。每組的比較次數為 x(x - 1) / 2，f[N] 為
最後決定出冠軍所需的比較次數，可以通過改變 x 的值，使 f[N] 改變。
給定 t, l, r。求區間 [l, r] 的每個 f[i]，按公式 t^0．f(l) + t^1．f(l+1) + ... + t^(r-l)．f(r) 求最小的值。

### Solution sketch

當 i 是質數，他只能分成一組，f[i] = i * (i - 1) / 2
當 i 是合數，f[i] = (i / minFactor) * f[minFactor] + f[i / minFactor]， minFactor 為 i 的最小質因數

<!-- more -->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 5000010;
bool is_prime[N];
long long int p[N];
long long int dp[N];
const long long int MOD = 1e9+7;
long long int t, l, r;
int cnt = 0;


void Isprime(){
	for (int i = 0; i < N; i++)
		is_prime[i] = true;
	is_prime[0] = is_prime[1] = false;

	for (int i = 2; i < N; i++){
		if (is_prime[i]){
			p[cnt++] = i;
			for (int j = i * 2; j < N; j += i){
				is_prime[j] = false;
			}
		}
	}
}

int main(){
	
	Isprime();
	
	scanf(" %lld %lld %lld", &t, &l, &r);
	
	dp[1] = 0;
	for (int i = 2; i < N; i++){
		if (is_prime[i])
			dp[i] = ((long long int)i * (i - 1) / 2) % MOD;
		else{
			for (int j = 0; j < cnt; j++){
				if (i % p[j] == 0){
					dp[i] = ((i / p[j]) * dp[p[j]] + dp[i / p[j]]) % MOD;
					break;
				}
			}
		}
	}
	
	long long int ans = 0;
	long long sqr = 1;
	for (int i = l; i <= r; i++){
		ans = (ans + sqr * dp[i]) % MOD;
		sqr = (sqr * t) % MOD;
	}
	
	printf("%lld\n", ans);

	return 0;
}
```
