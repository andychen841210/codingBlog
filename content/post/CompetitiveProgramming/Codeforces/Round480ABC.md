---
title: "Round480ABC"
date: 2018-05-29T10:55:41+08:00
draft: false
lastmod: 2018-05-29T10:55:41+08:00
tags: ["Math", "Greedy"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round480 Division 2](http://codeforces.com/contest/980)

## A

### Abridge problem statement

給定一堆的 links 和 pearls 問說可不可以任意移動 link 或 pearl，使得相鄰的兩個 pearl 之間的 link 數目相同

### Solution sketch

看 pearl 數可不可以整除 link 數，可以則輸出 YES，否則輸出 NO，注意當 pearl 為零的時候，輸出 YES

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	char a[1000];
	scanf(" %s", a);

	int len = strlen(a), cnt_link = 0, cnt_pearl = 0;
	for (int i = 0; i < len; i++){
		if (a[i] == '-')
			cnt_link++;
		else
			cnt_pearl++;
	}
	if (cnt_pearl == 0){
		printf("YES\n");
		return 0;
	}
	if (cnt_link % cnt_pearl != 0)
		printf("NO\n");
	else
		printf("YES\n");

	return 0;
}
```

## B

### Abridge problem statement

給定一個 4 * n 的地圖，n 為奇數，左上角 (1, 1) 為村子，要去右下角 (4, n) 的魚塘抓魚，左下角 (4, 1) 為村子，要去右上角 (1, n) 的魚塘抓魚，現在有 k 間旅館，問說如何設置這 k 間旅館，且旅館不能放在邊界上，使得左上角的村子到右下角的魚塘的最短路徑數與左下角的村子到右上角的魚塘的最短路徑數相等

### Solution sketch

考慮 k 為奇數偶數的情況

1. k 為偶數的時候，只需順著 col 擺滿即可
2. k 為奇數的時候，從 row = 1 然後從 col 中間開始往兩側擺，若擺滿還有剩，則往下一個 row 擺，也是一樣從中間往兩側擺，但中間那個不能擺放，因為當 k 為奇數的時候，若第一個 row 擺滿還有剩下了話，那剩下的數目一定為偶數

不會有 NO 的情況發生

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, k;
	scanf(" %d %d", &n, &k);

	char a[110][110];
	for (int i = 0; i < 4; i++)
		for (int j = 0; j < n; j++)
			a[i][j] = '.';
	
	if (k % 2 == 0){
		printf("YES\n");
		for (int j = 1; j < n - 1; j++){
			for (int i = 1; i < 3; i++){
				if (k == 0)
					break;
				a[i][j] = '#';
				k--;
			}
			if (k == 0)
				break;
		}

		for (int i = 0; i < 4; i++){
			for (int j = 0; j < n; j++){
				printf("%c", a[i][j]);
			}
			printf("\n");
		}
	}
	else{
		printf("YES\n");
		int x = n / 2, y = 0;
		while(1){
			if (k == 0)
				break;
			if (x + y >= n - 1)
				break;
			a[1][x + y] = '#';
			a[1][x - y] = '#';
			if (x + y == x - y)
				k--;
			else
				k -= 2;
			y++;
		}
		if (k > 0){
			y = 1;
			while(1){
				if (k == 0)
					break;
				a[2][x + y] = '#';
				a[2][x - y] = '#';
				k -= 2;
				y++;
			}
		}
		
		for (int i = 0; i < 4; i++){
			for (int j = 0; j < n; j++){
				printf("%c", a[i][j]);
			}
			printf("\n");
		}
	}
	
	return 0;
}
```

## C

### Abridge problem statement

給定 n 個數字代表像素的顏色，範圍 [0, 255]，你可以將值相鄰的若干個數字分成一組，之後在同一組內的數字都可以變成最小的那個，且要求每個組的長度不能超過 k，問如何分組使得新的 n 個數字照前述的規則變換後，使得新的序列的字典序最小

### Solution sketch

greedy 的想法：

1. 如果現在這個數沒有被分組了話，就往前找盡可能的小，小心，如果前面已經有組別，但加上目前這整個長度，若沒有超過 k，則這跟前面那個組合並
2. 如果這個數字已經被分組了話，則直接輸出這組最小的那個數

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n, k;
	scanf(" %d %d", &n, &k);

	int a[100010], color[256];
	for (int i = 0; i < n; i++){
		scanf(" %d", &a[i]);
	}
	
	fill(color, color + 256, -1);
	
	vector<int> ans;
	color[0] = 0;
	for (int i = 0; i < n; i++){
		if (color[a[i]] != -1){ // in the group
			ans.push_back(color[a[i]]);
			continue;
		}

		bool ch = false;
		for (int j = 0; j < k; j++){
			if (a[i] - j < 0)
				break;
			if (color[a[i] - j] != -1){
				int temp = a[i] - j - 1, cnt = 1;
				for (int l = temp; l >= 0; l--){
					if (color[l] != color[a[i] - j])
						break;
					else
						cnt++;
				}
				
				if (cnt + j <= k){ // 跟前面的 group 合併
					for (int l = 0; l < k; l++){
						if (a[i] - l >= 0 && color[a[i] - l] != color[a[i] - j])
							color[a[i] - l] = color[a[i] - j];
						else
							break;
					}
					ch = true;
				}
				else{
					for (int l = j - 1; l >= 0; l--){ // 往前找到第一個不是 -1 的合併
						if (a[i] - l < 0)
							break;
						color[a[i] - l] = a[i] - j + 1;
					}
					ch = true;
				}
			}
			else{
				if (j == k - 1){ // 往前找到第一個不是 -1 的合併
					for (int l = 0; l < k; l++){
						if (a[i] - l < 0)
							break;
						color[a[i] - l] = a[i] - j;
					}
				}
			}
			if (ch)
				break;
		}

		ans.push_back(color[a[i]]);
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