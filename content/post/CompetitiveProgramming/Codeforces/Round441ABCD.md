---
title: "Round441ABCD"
date: 2018-03-27T18:15:35+08:00
draft: false
lastmod: 2018-03-27T18:15:35+08:00
tags: ["Math", "Brute Force"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round441 Division 2](http://codeforces.com/contest/876)

## A

### Abridge problem statement

在圖上走 n - 1 次，使得總共走的距離最小

### Solution sketch

每次都走最小的那條邊即可

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, a, b, c;
	scanf(" %d", &n);
	scanf(" %d", &a);
	scanf(" %d", &b);
	scanf(" %d", &c);

	int now = 0, ans = 0;
	n--;
	while(n--){
		if (now == 0){
			if (a < b){
				ans += a;
				now = 1;
			}
			else{
				ans += b;
				now = 2;
			}
		}
		else if (now == 1){
			if (a < c){
				ans += a;
				now = 0;
			}
			else{
				ans += c;
				now = 2;
			}
		}
		else{
			if (b < c){
				ans += b;
				now = 0;
			}
			else{
				ans += c;
				now = 1;
			}
		}
	}

	printf("%d\n", ans);

	return 0;
}
```

## B

### Abridge problem statement

給定 n 個數，從中選出 k 個，使得兩兩的差值可以被 m 整除，若無法找到則輸出 No，若有了話則輸出 Yes，以及這 k 個數

### Solution sketch

將給定的 n 個數都 % m，將餘數相同的擺到同一個陣列裡面，如果這個陣列超過 k 個，輸出前 k 個即可，若全部的陣列都沒超過 k 個，則輸出 No，
因為相同的數若 % m 的餘數相同了話，則想個相減後的值必定能被 m 整除

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	
	int n, k, m;
	vector<int> v[100010];

	scanf(" %d %d %d", &n, &k, &m);

	for (int i = 0; i < n; i++){
		int x;
		scanf(" %d", &x);
		v[x % m].push_back(x);
	}

	for (int i = 0; i < 100010; i++){
		if (v[i].size() >= k){
			printf("Yes\n");
			for (int j = 0; j < k; j++){
				if (j == 0)
					printf("%d", v[i][j]);
				else
					printf(" %d", v[i][j]);
			}
			printf("\n");

			return 0;
		}
	}

	printf("No\n");

	return 0;
}
```

## C

### Abridge problem statement

給定一個數 n，問是否存在 x，使得 x 加上 x 的個別位數會等於 n，若有了話，則先輸出有幾個符合的，接著由小到大輸出符合的 x，
若沒有了話，則輸出 0

### Solution sketch

因為最大的 n 只有 1000000000，因此 x 最大個個別位數相加不會超過 100，因為 99999999 的個別位數和為 72，
因此可以搜大一點沒關係，接著可以把題目換成數學式，n = x + (x 的個別位數和)，移項及為 n - (x 的個別位數和) = x，
就可以搜個別位數和 i，從 0 到 100，將 input 的 n 減去 i 及為 x，在檢查 x 個別位數加起來是否等於現在搜的 i，
若相等就 x 塞入答案陣列當中，最後將答案陣列中的東西輸出即可

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n;
	vector<int> v;
	scanf(" %d", &n);
	
	for (int i = 0; i < 100; i++){
		int temp = n - i, cnt = 0;
		if (temp < 0)
			break;
		while(temp){
			cnt += temp % 10;
			temp /= 10;
		}

		if (cnt == i)
			v.push_back(n - i);
	}
	
	sort(v.begin(), v.end());
	printf("%d\n", v.size());
	for (int i = 0; i < v.size(); i++){
		printf("%d\n", v[i]);
	}

	return 0;
}	
```

## D

### Abridge problem statement

題目很難看懂，基本的意思是有 n 個位置，有 n 次操作，每次操作在第 p[i] 的位置上放上一枚硬幣，然後每次放上一枚硬幣以後，
要從左往右掃一遍，，如果一個硬幣右邊沒有硬幣，則把這個硬幣移到右邊有硬幣的位置或者移到最右邊，然後再去看下一個硬幣，一直重複，
直到當前的所有硬幣都移到最右邊為止，問每次操作，需要多少步驟才能移到最後，初始狀態算一步

### Solution sketch

只需要由右邊往左邊看，是否最右邊的位置是否有被硬幣佔住，若新放進來的硬幣沒有佔住最右邊的格子了話，則為上一次移動數加 1，
若有被佔住了話，則往前找到離最右邊最近的沒被佔住的格子，當前移動個數減去最右邊有被佔住的個數即可

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, ans = 1, last;
	int a[300010] = {0};
	
	scanf(" %d", &n);
	last = n;
	
	printf("1");
	for (int i = 0; i < n; i++){
		int x;
		scanf(" %d", &x);
		
		a[x] = 1;
		ans += 1;

		while(a[last]){
			ans -= 1;
			last -= 1;
		}

		printf(" %d", ans);
	}
	printf("\n");

	return 0;
}
```