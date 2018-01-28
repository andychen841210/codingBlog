---
title: "Round411ABCD"
date: 2018-01-28T21:25:10+08:00
draft: false
lastmod: 2018-01-28T21:25:10+08:00
tags: ["Math", "String"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round411 Division 2](http://codeforces.com/contest/805)

## A

### Abridge problem statement

給定 l 跟 r，問哪個數字可以在這個區間中整除最多的數字，數字 1 除外

### Solution sketch

若 r - l 大於等於 1 了話，則表示每多兩個數就會有一個偶數，因此，這時候 2 會是可以整除最多的，否則了話 r - l 則為 0，因此輸出 r 或 l 即可

<!-- more -->

### AC code
```python
l, r = map(int,input().split())
s = r - l
if s >= 1:
	print("2")
else:
	print(l)
```

## B

### Abridge problem statement

給定一個數字 n，需要用 a, b, c 三個字元組出長度為 n 的字串，且在任意 substring 長度為 3 的情況下，不可出現回文，且需要讓 c 出現的次數最少

### Solution sketch

以 'abba' 去擴增字串，'abbaabba....'，就不會使用到 c ，且保證任意長度為 3 的 substring 不會出現回文

<!-- more -->

### AC code
```python
n = int(input())
ans = ''
for i in range(0, n):
	if i % 4 == 0:
		ans += 'a'
	elif i % 4 == 1:
		ans += 'b'
	elif i % 4 == 2:
		ans += 'b'
	else:
		ans += 'a'
print(ans)
```

## C

### Abridge problem statement

給定一個數字 n，表示有 1 到 n 間學校，需要使用最少的費用通過所有的學校，經過任兩間學校的費用為 (i + j) mod (1 + n)

### Solution sketch

小配大，大再配小，不斷來回直到通過所有學校

<!-- more -->

### AC code
```python
n = int(input())
mod = 1 + n
ans = 0
cnt = 1
big = n
small = 1
while cnt != n:
	if cnt % 2 == 1:
		ans += (small + big) % mod
		small += 1
		cnt += 1
	else:
		ans += (small + big) % mod
		big -= 1
		cnt += 1
print(ans)
```


## D

### Abridge problem statement

給定一個由 a 和 b 組成的字串，若遇到字串中有出現 ab 的部分，則可以把它換成 bba，問最少需要幾步把他換到不能再換為止

### Solution sketch

a 遇到 b，則需往右推 cnt_b 個位置，每往右推一次，則會多一個 b，因為 ab -> bba，因此只需從後面算回來，遇到 b，則 cnt_b 加 1，若遇到 a，則把答案加上現在 cnt_b 的個數，然後 cnt_b 乘以 2，因為每換一次會多出一個 b

<!-- more -->

### AC code
```python
inp = input()
cnt_b = 0
ans = 0
mod = 1000000000 + 7
for i in inp[::-1]:
	if i == 'b':
		cnt_b += 1
	else:
		ans = (ans + cnt_b) % mod
		cnt_b = (cnt_b * 2) % mod
print(ans)
```
