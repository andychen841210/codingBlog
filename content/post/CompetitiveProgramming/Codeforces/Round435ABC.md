---
title: "Round435ABC"
date: 2018-04-03T19:33:06+08:00
draft: false
lastmod: 2018-04-03T19:33:06+08:00
tags: ["Dfs"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round435 Division 2](http://codeforces.com/contest/862)

## A

### Abridge problem statement

給定 n 個不同的非負整數與 x，可以做的操作有加入或刪除任意的數字，問最少要幾步驟可以讓這串數字第一個缺少的數字是 x

### Solution sketch

將比 x 小的數字補起來，若有 x 則在刪除 x 即可

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, x, inp;
	int a[1000] = {0};

	scanf(" %d %d", &n, &x);
	for (int i = 0; i < n; i++){
		scanf(" %d", &inp);
		a[inp] = 1;
	}
	
	int cnt = 0;
	for (int i = 0; i < x; i++){
		if (a[i] == 0)
			cnt++;
	}
	if (a[x] == 1)
		cnt++;
	
	printf("%d\n", cnt);

	return 0;
}
```

## B

### Abridge problem statement

給定 n 個點，n - 1 條邊，他們構成一個二分圖，問最多可以加幾條邊，使得最後得到的圖還是二分圖

### Solution sketch

直接跑 dfs 計算一個 set 的個數，接著迭代另一個 set 中的點，看每個點還可以連多少個邊，加總起來及為答案，注意要開 long long

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> v[100100], s;
int n;
int s1 = 0, s2 = 0;
bool vis[100100];

void dfs(int x, int ss){
	vis[x] = true;
	
	if (ss == 0){
		s.push_back(x);
		s1++;
	}
	else
		s2++;
	
	for (int i = 0; i < v[x].size(); i++){
		if (vis[v[x][i]] == false){
			if (ss == 0)
				dfs(v[x][i], 1);
			else
				dfs(v[x][i], 0);
		}
	}
}

int main(){

	scanf(" %d", &n);
	
	for (int i = 0; i < n - 1; i++){
		int a, b;
		scanf(" %d %d", &a, &b);
		a--; b--;
		v[a].push_back(b);
		v[b].push_back(a);
	}

	dfs(0, 0);

	long long int ans = 0;
	for (int i = 0; i < s.size(); i++){
		ans += s2 - v[s[i]].size();
	}

	printf("%lld\n", ans);

	return 0;
}
```

## C

### Abridge problem statement

給定 n 和 x，問說可不可以用 n 個數字 xor 起來組出 x，n 個數字都要小於 1e6

### Solution sketch

分幾種狀況討論

1. 當 n 等於 1 時，就直接輸出 x
2. 當 n 等於 2 且 x 等於 0 時，則輸出 NO
3. 當 n 等於 2 且 x 不等於 0 時，則輸出 0 跟 x
4. 剩下情況，先將前 n - 3 個做 xor 得到 y，若 y 等於 x 了話，剩下三個則為 pw、pw * 2、pw ^ (pw * 2)，
pw 為一個超過最大 x(1e5) 以及小於 1e6 的數字，若 y 不等於 x 了話，則剩下三個則為 0、pw、pw ^ y ^ x

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int a[1000100];

int main(){
	int n, x;
	vector<int> v;

	scanf(" %d %d", &n, &x);
	
	if (n == 1)
		printf("YES\n%d\n", x);
	else if (n == 2){
		if (x == 0)
			printf("NO\n");
		else
			printf("YES\n0 %d\n", x);
	}
	else{
		printf("YES\n");
		
		int x_or = 0, pw = 1 << 18;
		vector<int> ans;
		for (int i = 1; i <= n - 3; i++){
			if (i == 1)
				x_or = i;
			else
				x_or ^= i;
			ans.push_back(i);
		}

		if (x_or == x){
			ans.push_back(pw); ans.push_back(pw * 2); ans.push_back(pw ^ (pw * 2));
			for (int i = 0; i < ans.size(); i++){
				if (i == 0)
					printf("%d", ans[i]);
				else
					printf(" %d", ans[i]);
			}
			printf("\n");
		}
		else{
			ans.push_back(0); ans.push_back(pw); ans.push_back(pw ^ x ^ x_or);
			for (int i = 0; i < ans.size(); i++){
				if (i == 0)
					printf("%d", ans[i]);
				else
					printf(" %d", ans[i]);
			}
			printf("\n");
		}
	}

	return 0;
}
```