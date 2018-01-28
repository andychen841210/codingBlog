---
title: "GCJ2017QualificationRoundB"
date: 2018-01-28T21:34:41+08:00
draft: false
lastmod: 2018-01-28T21:34:41+08:00
tags: ["Greedy"]
categories: ["Competitive Programming", "Google Code Jam"]
---
## [Tidy Numbers](https://code.google.com/codejam/contest/3264486/dashboard#s=p1)

### Abridge problem statement

定義一個數 x 是 tidy number 若且唯喏 x 的 digits 成單調遞增（non-descending）。例如 1122334, 379 就是 tidy number，而 635, 5773 就不是。現給定一正整數 N，求 1 ~ N 中最大的 tidy number 是哪個數？

### Solution sketch

可觀察出一些特性:
1. 若 N 本身就是 tidy，就答案就是 N
2. 否則，找出前綴最長的單調遞增 P，若 P 的尾數沒有連續一樣的數字，例如 1256XXX，則 P 的最後一位減去 1， P 後面的數字都補上 9，若 P 的尾數有連續一樣的數字了話，則找到最前面第一個一樣的數字減 1 ，剩下後面的數字都補上 9
3. 最後檢查開頭是否變成 0，若變成 0，則把開頭的 0 拔掉

<!-- more -->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

char s[100];

int main(){
	int T;
	
	scanf(" %d", &T);
	
	for (int k = 0; k < T; k++){
		scanf(" %s", s);
		
		int l = strlen(s);
		bool t = true;
		for (int i = 1; i < l; i++){
			if (s[i] < s[i - 1]){
				t = false;
				break;
			}
		}

		if (t){
			printf("Case #%d: %s\n", k + 1, s);
			continue;
		}
		
		int same = 0, first = 0, change = 0;
		for (int i = 1; i < l; i++){
			if (s[i] < s[i - 1]){
				if (first == 0){
					change = i - 1;
					same = -1;
				}
				else{
					change = i - 1;
				}
				break;
			}
			if (s[i] == s[i - 1] && first == 0){
				same = i - 1;
				first = 1;
				change = i;
			}
			if (s[i] > s[i - 1]){
				change = i;
				first = 0;
			}
		}

		if (same == -1){
			for (int i = change; i < l; i++){
				if (i == change)
					s[i] = s[i] - 1;
				else
					s[i] = '9';
			}
		}
		else{
			for (int i = same; i < l; i++){
				if (i == same)
					s[i] = s[i] - 1;
				else
					s[i] = '9';
			}
		}

		int st = 0;
		for (int i = 0; i < l; i++){
			if (s[i] != '0'){
				st = i;
				break;
			}
		}

		printf("Case #%d: ", k + 1);
		for (int i = st; i < l; i++)
			printf("%c", s[i]);
		printf("\n");

	}

	return 0;
}
```