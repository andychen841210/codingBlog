---
title: "GCJ2017QualificationRoundA"
date: 2018-01-28T21:34:29+08:00
draft: false
lastmod: 2018-01-28T21:34:29+08:00
tags: ["Greedy"]
categories: ["Competitive Programming", "Google Code Jam"]
---
## [Oversized Pancake Flipper](https://code.google.com/codejam/contest/3264486/dashboard#s=p0)

### Abridge problem statement

字串只包含 -, +，定義一次反轉代表將 S 中一個（連續）長度為 K 的區間中的字元反轉：- 變成 +，+ 變成 -。請問最少要反轉幾次才能使整個字串變成全部都為 +。

### Solution sketch

直接轉

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int k;
char s[10000];

int main(){
	int T;
	scanf(" %d", &T);
	
	for (int t = 0; t < T; t++){
		int a[10000] = {0};

		scanf(" %s", s);
		scanf(" %d", &k);
		
		int ans = 0, l = strlen(s);
		for (int i = 0; i < l; i++){
			if (s[i] == '-'){
				if (i + k > l)
					continue;
				for (int j = i; j < i + k; j++){
					if (s[j] == '+')
						s[j] = '-';
					else
						s[j] = '+';
				}
				ans++;
			}
		}

		for (int i = 0; i < l; i++){
			if (s[i] == '-'){
				ans = -1;
				break;
			}
		}

		
		if (ans == -1)
			printf("Case #%d: IMPOSSIBLE\n", t + 1);
		else
			printf("Case #%d: %d\n", t + 1, ans);
	}
	return 0;
}
```
