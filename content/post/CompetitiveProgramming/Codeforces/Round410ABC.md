---
title: "Round410ABC"
date: 2018-01-28T21:27:07+08:00
draft: false
lastmod: 2018-01-28T21:27:07+08:00
tags: ["Math", "String"]
categories: ["Competitive Programming", "Codeforces", "Division 2"]
---
## [Codeforces Round410 Division 2](http://codeforces.com/contest/798)

## A

### Abridge problem statement

給定一個字串，問說可不可以換字串中一個字，使這個字串變成回文

### Solution sketch

直接掃過去看有沒有辦法換一個字讓他變成回文，若本身就是回文了話，若字串長度為偶數了話答案為 NO，因為一定要換一個字

<!--more-->

### AC code
```python
s = input()
l = 0
r = len(s) - 1
cnt = 0
while l < r:
	if s[l] != s[r]:
		cnt += 1
	l += 1
	r -= 1
if cnt == 0:
	if len(s) % 2 == 1:
		cnt = 1
if cnt == 1:
	print("YES")
else:
	print("NO")
```

## B

### Abridge problem statement

給定 n 個字串，問說最少要動幾步讓每個字串都變成一樣，若沒辦法讓每個字串都變成一樣，則輸出 -1

### Solution sketch

讓每個字串都當最終字串直接做，然後找最少的

<!-- more -->

### AC code
```python
n = int(input())
a = []
ans = 1000000
for i in range(0, n):
	s = input()
	a.append(s)
for i in range(0, n):
	cnt = 0
	for j in range(0, n):
		if i == j:
			continue
		for k in range(0, len(a[i])):
			ch = True
			for l in range(0, len(a[i])):
				if a[i][l] != a[j][(k + l) % len(a[i])]:
					ch = False
					break
			if k == len(a[i]) - 1 and ch == False:
				print(-1)
				exit(0)
			if ch:
				cnt += k
				break
	ans = min(ans, cnt)
print(ans)
```

## C

### Abridge problem statement

給定一堆數字，問說最少動幾步讓所有數字的 gcd > 1，若有答案了話，輸出 YES 跟最少的步數，若沒有了話，則輸出 NO，將 ai 跟 ai+1 變成 ai - ai + 1 跟 ai + ai+1，視為動一步

### Solution sketch

觀察一下會發現，其實答案中不會出現 NO，這個答案，接著若全部都為偶數，則沒有問題，若序列上出現連續兩個奇數了話，則需做一次操作讓他們都變成偶數，若一奇一偶，則須做兩次操作讓他們都變成偶數

需注意，若一開始 gcd 就大於 1 了話，則直接輸出答案

<!-- more -->

### AC code
```python
def gcd (a, b):
	while b:
		temp = a % b
		a = b
		b = temp
	return a

n = int(input())
a = list(map(int, input().split()))
ans = 0
g = a[0]

for i in range(1, n):
	g = gcd(g, a[i])
	if a[i] % 2 != 0:
		if a[i - 1] % 2 != 0:
			ans += 1
			a[i] = 2
			a[i - 1] = 2

for i in a:
	if i % 2 != 0:
		ans += 2

print("YES")

if g > 1:
	print(0)
else:
	print(ans)
```
