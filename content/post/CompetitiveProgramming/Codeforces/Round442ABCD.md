---
title: "Round442ABCD"
date: 2018-03-30T22:51:22+08:00
draft: false
lastmod: 2018-03-30T22:51:22+08:00
tags: ["String", "Dp", "Bfs"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round442 Division 2](http://codeforces.com/contest/877)

## A

### Abridge problem statement

給定一個字串，問字串中有沒有出現 "Danil", "Olya", "Slava", "Ann" and "Nikita" 其中一個名字，而這個名字中只能在字串中出現一次

### Solution sketch

直接掃一遍

<!--more-->

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	char s[110], a[5][10] = {"Danil", "Olya", "Slava", "Ann", "Nikita"};

	scanf(" %s", s);
	
	int cnt = 0;
	for (int i = 0; i < strlen(s); i++){
		for (int j = 0; j < 5; j++){
			if (s[i] == a[j][0]){
				int ch = 1;
				for (int k = 0; k < strlen(a[j]); k++){
					if (s[i + k] != a[j][k]){
						ch = 0;
						break;
					}
				}

				if (ch)
					cnt += 1;
			}
		}
	}

	if (cnt == 1)
		printf("YES\n");
	else
		printf("NO\n");
	
	return 0;
}
```

## B

### Abridge problem statement

給定一個由 a、b 組成的字串，將字串切成三個字串，第一個跟第三個字串只能包含 a，第二個字串只能包含 b，可以是空字串，
可以對字串刪除任意字元，問如何讓字串切成三等分後，合起來的長度最長，字元不能改變他的順序，但可以刪除

### Solution sketch

prefix[i] 表示第 i 個之前包含多少個 a，sufix[i] 表示第 i 個之後包含多少個 i，
a_cnt 為 a 的個數，b_cnt 為 b 的個數，b1 為在 i 之前有多少個 b，b2 為在 j 之後有多少個 b
接著分 5 種 case 討論，取其中的最大值即可

1. 全部都是 a 的時候
2. 全部都是 b 的時候
3. aa...aa + bb...bb + aa...aa，prefix[i] + sufix[j] + b_cnt - b1 - b2
4. bb...bb + aa...aa，第一個是空的，b_cnt + sufix[j] - b2
5. aa...aa + bb...bb，第三個是空的，prefix[i] + b_cnt - b1

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	char s[5010];
	scanf(" %s", s);

	int l = strlen(s);
	int prefix[5010] = {0}, sufix[5010] = {0};
	int mx = 0;
	for (int i = 0; i < l; i++){
		if (s[i] == 'a'){
			mx += 1;
		}
		prefix[i] = mx;
	}

	mx = 0;
	for (int i = l - 1; i >= 0; i--){
		if (s[i] == 'a'){
			mx += 1;
		}
		sufix[i] = mx;
	}

	int a_cnt = 0, b_cnt = 0, ans = 0;
	for (int i = 0; i < l; i++){
		if (s[i] == 'a')
			a_cnt += 1;
		if (s[i] == 'b')
			b_cnt += 1;
	}

	int b1 = 0, b2 = 0;
	for (int i = 0; i < l; i++){
		b2 = 0;
		if (s[i] == 'b')
			b1++;
		for (int j = l - 1; j > i; j--){
			if (s[j] == 'b')
				b2++;
			ans = max(ans, prefix[i] + sufix[j] + b_cnt - b1 - b2);
			ans = max(ans, sufix[j] + b_cnt - b2);
			ans = max(ans, prefix[i] + b_cnt - b1);
		}
	}

	printf("%d\n", max(ans, max(a_cnt, b_cnt)));

	return 0;
}
```

## C

### Abridge problem statement

給定一個 1 * n 的 cells，在這些 cells 上會有任意台坦克，你現在可以從天空中丟炸彈下去任意個 cells，
當坦克第一次被炸到的會移動到隔壁的 cell，當第二次被炸到的時候坦克就會壞掉，問說最少需要多少個炸彈可以把全部的坦克炸壞

### Solution sketch

先炸偶數格子，再炸奇數，再炸偶數，為什麼不先炸奇數，再偶數，再奇數，因為在格子是奇數的時候會多炸一次

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	int n;
	scanf(" %d", &n);
	
	printf("%d\n", n / 2 * 2 + n - n / 2);
	for (int i = 2; i <= n; i += 2){
		if (i == 2)
			printf("%d", i);
		else
			printf(" %d", i);
	}
	
	for (int i = 1; i <= n; i += 2)
		printf(" %d", i);
	
	for (int i = 2; i <= n; i += 2)
		printf(" %d", i);
	
	printf("\n");

	return 0;
}
```

## D

### Abridge problem statement

給定一張 n * m 的地圖，給定起點和終點，一個人從起點出發，每秒可以像一個方向走 1 ~ k 中的任意步數，問從起點到終點最少需花多少時間，
如果不能走到，則輸出 -1

### Solution sketch

這跟一般的走迷宮不太一樣，因為他每秒可以走多步，所以我們只要去枚舉每個方向及該方向上走的步數
bfs，要注意剪枝否則會 TLE，我們仍用 vis 來表示是否走過，只是多一個維度是用來表示這個方向是否走過

### AC code
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m, k, x1, x2, y11, y2;
char a[1010][1010];
bool vis[2][1010][1010];
int ans;
bool ch = false;
int dir_y[4] = {0, 1, 0, -1}, dir_x[4] = {-1, 0, 1, 0};

struct INFO{
	int x, y, cnt;
};

int main(){

	scanf(" %d %d %d", &n, &m, &k);

	for (int i = 0; i < n; i++)
		for (int j = 0; j < m; j++)
			scanf(" %c", &a[i][j]);
	
	scanf(" %d %d %d %d", &x1, &y11, &x2, &y2);
	x1--; x2--; y11--; y2--;
	
	if (x1 == x2 && y11 == y2){
		printf("0\n");
		return 0;
	}

	queue<INFO> q;
	q.push(INFO{x1, y11, 0});

	while(!q.empty()){
		INFO info = q.front(); q.pop();
		if (info.x == x2 && info.y == y2){
			printf("%d\n", info.cnt);
			return 0;
		}

		for (int i = 0; i < 4; i++){
			for (int j = 1; j <= k; j++){
				int temp_x = info.x + dir_x[i] * j, temp_y = info.y + dir_y[i] * j;
				if (temp_x < 0 || temp_x >= n || temp_y < 0 || temp_y >= m)
					break;
				if (a[temp_x][temp_y] == '#')
					break;
				if (vis[i & 1][temp_x][temp_y])
					break;

				vis[i & 1][temp_x][temp_y] = true;
				q.push(INFO{temp_x, temp_y, info.cnt + 1});
			}
		}
	}

	printf("-1\n");

	return 0;
}
```