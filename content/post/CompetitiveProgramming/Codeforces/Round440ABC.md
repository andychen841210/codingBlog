---
title: "Round440ABC"
date: 2018-03-22T23:24:52+08:00
draft: false
lastmod: 2018-03-22T23:24:52+08:00
tags: ["Math", "Greedy"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round440 Division 2](http://codeforces.com/contest/872)

## A

### Abridge problem statement

給定兩個非零 digit 的 list，求出最小的 pretty number，pretty number 定義為至少一個 digit 從第一個 list，至少一個 digit 從第二個 list

### Solution sketch

先檢查兩個 list 是否有重複的數字，若有了話找出最小的個位數就是答案，否則取兩個 list 當中個別最小的值拿出來，較小的擺十位數，較大的擺在個位數就是答案

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m;
vector<int> a, b;

int main(){

	scanf(" %d %d", &n, &m);
	
	int x;
	for (int i = 0; i < n; i++){
		scanf(" %d", &x);
		a.push_back(x);
	}
	for (int i = 0; i < m; i++){
		scanf(" %d", &x);
		b.push_back(x);
	}

	sort(a.begin(), a.end());
	sort(b.begin(), b.end());
	
	int ans = -1;
	for (int i = 0; i < n; i++){
		for (int j = 0; j < m; j++){
			if (a[i] == b[j]){
				ans = a[i];
				printf("%d\n", ans);
				return 0;
			}
		}
	}
	
	printf("%d%d\n", min(a[0], b[0]), max(a[0], b[0]));

	return 0;
}
```

## B

### Abridge problem statement

給定 n, k 表示有一個包含 n 個 integers 的 array，則需要把他分成 k 個 subsegments，要求取 maximum of minimums on the subsegments，
maximum of minimums on the subsegments 定義為每個 subsegment 中的最小值，然後在取這全部最小值中的最大值，要讓這個值越大越好

### Solution sketch

只會有 3 種 case

1. 當 k = 1 時，只會有一個 segment 所以答案就是最小值
2. 當 k = 2 時，先對 list 從左到右做 prefix 的 min，接著從右到左找目前最小的值是誰，然後跟 n - 1 的 prefix min 值比較誰比較大，
全部掃過去後找到最大的就是答案
3. 當 k >= 3 時，找最大值即可，因為切為 3 段以上，最大的那個值一定可以自己做一個 subsegment

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, k, ans;
	int a[100010];

	scanf(" %d %d", &n, &k);
	
	for (int i = 0; i < n; i++)
		scanf(" %d", &a[i]);
	
	if (k == 1){
		ans = a[0];
		for (int i = 1; i < n; i++)
				ans = min(ans, a[i]);

		printf("%d\n", ans);
	}
	else if (k == 2){
		int pre_min[100010];
		pre_min[0] = a[0];
		
		for (int i = 1; i < n; i++)
			pre_min[i] = min(pre_min[i - 1], a[i]);

		int mn = a[n - 1];
		ans = mn;
		for (int i = n - 2; i >= 1; i--){
			mn = min(mn, a[i]);
			ans = max(ans, max(mn, pre_min[i - 1]));
		}
		ans = max(ans, max(mn, pre_min[0]));

		printf("%d\n", ans);
	}
	else{
		ans = a[0];
		for (int i = 1; i < n; i++)
			ans = max(ans, a[i]);

		printf("%d\n", ans);
	}
	return 0;
}
```

## C

### Abridge problem statement

給定一個數 n，You are to represent n as a sum of maximum possible number of composite summands and print this maximum number, or print -1, if there are no such splittings.
An integer greater than 1 is composite, if it is not prime, i.e. if it has positive divisors not equal to 1 and the integer itself.

### Solution sketch

1. 遇到 1, 2, 3, 5, 7, 11 則輸出 -1
2. 遇到 6 則輸出 1
3. 遇到奇數，因為 9 是第一個合法的 summand，因此將此數扣掉 9，答案個數加 1，接著就和偶數的 case 相同
4. 遇到偶數，因為 4 和 6 是合法的 summand，且任意的偶數都可以由 4 和 6 組成，因此，先看是否能被 4 整除，如果可以就直接輸出整除的個數，若不行，則扣掉一個 6 答案加 1，扣完 6 的值再除以 4 加到答案即可

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int prime[6] = {1, 2, 3, 5, 7, 11};

int main(){
	int n;
	scanf(" %d", &n);

	while(n--){
		int x;
		scanf(" %d", &x);
		
		bool ch = false;
		for (int i = 0; i < 6; i++){
			if (x == prime[i]){
				printf("%d\n", -1);
				ch = true;
				break;
			}
		}

		if (ch)
			continue;
		
		int ans = 0;
		if (x % 2 == 1){
			x -= 9;
			ans++;
			if (x % 4 == 0)
				printf("%d\n", ans + x / 4);
			else
				printf("%d\n", ans + (x - 6) / 4 + 1);
		}
		else{
			if (x % 4 == 0)
				printf("%d\n", x / 4);
			else
				printf("%d\n", (x - 6) / 4 + 1);
		}
	}

	return 0;
}
```