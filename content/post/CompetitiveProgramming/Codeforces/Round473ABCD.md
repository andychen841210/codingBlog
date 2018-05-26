---
title: "Round473ABCD"
date: 2018-05-26T19:20:20+08:00
draft: false
lastmod: 2018-05-26T19:20:20+08:00
tags: ["Math", "Greedy"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round473 Division 2](http://codeforces.com/contest/959)

## A

### Abridge problem statement

Mahmoud 和 Ehab 在玩一個遊戲，給定一個數字 n，每次輪到 Mahmoud 時，剩下的數字必須是偶數，否則 Mahmoud 就輸了，輪到 Ehab 時，剩下的數字必須是奇數，否則Ehab 就輸了，每個人每次的操作可以拿任意 1 到 n 之間的數，問說給定一個數字 n，最後贏的會是誰

### Solution sketch

直接判奇偶數

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n;
	scanf(" %d", &n);

	if (n % 2 == 0)
		printf("Mahmoud\n");
	else
		printf("Ehab\n");

	return 0;
}
```

## B

### Abridge problem statement

給定 n 個不同的字，每個字都有屬於自己的分數和 group，在同一個 group 當中的字可以互換，然後給定一串文字，問你說這一串文字最小的分數加總是多少

### Solution sketch

記錄每一個 group 中最小的值是多少，然後將給定的字串，每個字都換成那個 group 中最小的分數，最後加總起來就是答案了

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

char a[100100][25];
int b[100100];

int main(){
	int n, k, m;
	scanf(" %d %d %d", &n, &m, &k);
	
	map<string, int> mp1; // string -> group
	map<int, int> mp2; // group -> min cost
	for (int i = 0; i < n; i++)
		scanf(" %s", a[i]);
	
	for (int i = 0; i < n; i++)
		scanf(" %d", &b[i]);
	
	for (int i = 0; i < m; i++)
		mp2[i] = 0x3f3f3f3f;

	for (int group_id = 0; group_id < m; group_id++){
		int group_cnt;
		scanf(" %d", &group_cnt);
		for (int j = 0; j < group_cnt; j++){
			int x;
			scanf(" %d", &x);
			x--;
			mp1[a[x]] = group_id;
			mp2[group_id] = min(mp2[group_id], b[x]);
		}
	}
	
	long long int ans = 0;
	for (int i = 0; i < k; i++){
		char x[25];
		scanf(" %s", x);
		ans += mp2[mp1[x]];
	}

	printf("%lld\n", ans);
	
	return 0;
}
```

## C

### Abridge problem statement

給定一個錯誤的算法，問你說有沒有辦法找出不符合這個算法的樹，如果沒辦法則輸出 -1，以及符合這個算法的樹，如果有則輸出這個不符合的樹，以及符合這個算法的樹

### Solution sketch

觀察當 n <= 5 的時候，這個算法都會成立，當 n > 5 的時候，不成立的情況是 root 下面接一半個點，剩下一半接在第三層，成立的情況就是 root 下面接剩下全部的點

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n;
	
	scanf(" %d", &n);

	if (n <= 5){
		printf("-1\n");
		for (int i = 0; i < n - 1; i++)
			printf("1 %d\n", i + 2);
	}
	else{
		int x = n / 2;
		if (n % 2 == 0){
			// fail
			for (int i = 0; i < n / 2; i++){
				printf("1 %d\n", i + 2);
			}
			for (int i = 0; i < n / 2 - 1; i++){
				printf("2 %d\n", i + n / 2 + 2);
			}

			// correct
			for (int i = 0; i < n - 1; i++)
				printf("1 %d\n", i + 2);
		}
		else{
			// fail
			for (int i = 0; i < n / 2; i++){
				printf("1 %d\n", i + 2);
				printf("2 %d\n", i + 2 + n / 2);
			}

			// correct
			for (int i = 0; i < n - 1; i++)
				printf("1 %d\n", i + 2);
		}
	}

	return 0;
}
```

## D

### Abridge problem statement

給出一個長度為 n 的陣列 a，要你構造出滿足下面三個條件且長度也為 n 的陣列 b:

1. 陣列 b 的字典序要大於等於 a
2. 陣列 b 中的任意元素大於等於 2
3. 陣列 b 中的任意兩個元素要互質

同時題目要求 b 的字典序盡可能的小

### Solution sketch

能不改就不改，但是一但改了以後，後面的都要改，具體而言，就是從前往後掃，如果當前位置的數不與前面的衝突了話，我們就不動他。但是如果有一個數衝突了，那根據題意，我們得換一個更大的數，這時整體的字典序就比原本的還大了，因此後面的數就要盡可能的小

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAX_N = 1500000;
vector<int> num[MAX_N];
bool used[MAX_N], divisor[MAX_N];
set<int> s;

void init(){
	fill(divisor, divisor + MAX_N, true);
	divisor[0] = divisor[1] = false;
	for (int i = 2; i < MAX_N; i++){
		if (divisor[i]){
			for (int j = i; j < MAX_N; j += i){
				divisor[j] = false;
				num[j].push_back(i); // j 這個數的因數有誰
			}
		}
		s.insert(i); // 2 ~ n 的值都可以
	}
}

int main(){
	init();
	
	int n, cnt = 0;
	scanf(" %d", &n);
	
	bool is_big = false;
	vector<int> ans;
	for (int i = 0; i < n; i++){
		int a, b;
		scanf(" %d", &a);
		b = *s.begin();
		if (!is_big){
			b = *s.lower_bound(a); // 找目前合法且第一個大於等於 b 的數
			if (a != b)
				is_big = true;
		}

		ans.push_back(b);
		for (auto j: num[b]){ // 將 b 有的因數從 s 中移除
			if (!used[j]){
				for (int k = j; k < MAX_N; k += j){
					used[k] = true;
					s.erase(k);
				}
			}
		}
	}

	for (int i = 0; i < n; i++){
		if (i == 0)
			printf("%d", ans[i]);
		else
			printf(" %d", ans[i]);
	}
	printf("\n");

	return 0;
}
```