---
title: "Round514C"
date: 2018-10-23T23:27:30+08:00
draft: false
lastmod: 2018-10-23T23:27:30+08:00
tags: ["Math"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round514 Division 2](http://codeforces.com/contest/1059)

## C

### Abridge problem statement

給定一個 n，第一次輸出 1 ~ n 的 gcd，接著從中刪除一個數，接著數出這 n - 1 個數的 gcd，再刪除一個數，再輸出剩下數的 gcd，直到輸出 n 個 gcd，要求輸出字典序最大的數列

### Solution sketch

對於一個 n，為了使字典序最大，所以越早輸出 2 越好，因此先把奇數全部去掉，並輸出 (n + 1) / 2 個 1，接著剩下的偶數，就可以將他們全部除以 2，接著刪除掉奇數的部分，並且輸出 2，因為他們都除過 2 了，接著再對剩下的偶數做同樣的操作，要特別判斷 n == 3 的情況即可

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n;
	scanf(" %d", &n);

	int mul = 1;
	while(n){
		if (n == 1){
			printf("%d\n", mul);
			return 0;
		}
		else if (n == 2){
			printf("%d %d\n", mul, mul * 2);
			return 0;
		}
		else if (n == 3){
			printf("%d %d %d\n", mul, mul, mul * 3);
			return 0;
		}
		else{
			for (int i = 0; i < n / 2 + n % 2; i++)
				printf("%d ", mul);
			
			mul *= 2;
			n /= 2;
		}
	}

	return 0;
}
```