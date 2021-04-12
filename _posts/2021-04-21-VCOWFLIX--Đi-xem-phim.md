---
layout: post
title: VCOWFLIX - Đi xem phim
subtitle: vnspoj
tags: [recursion]
---
Nông dân John đang đưa các con bò của anh ta đi xem phim! Xe tải của anh ta thì có sức chứa có hạn thôi, là C (100≤C≤5000) kg, anh ta muốn đưa 1 số con bò đi xem phim sao cho tổng khối lượng của đống bò này là lớn nhất, đồng thời xe tải của anh ta vẫn chịu được.

Cho N (1≤N≤16) con bò và khối lượng Wi của từng con, hãy cho biết khối lượng bò lớn nhất mà John có thể đưa đi xem phim là bao nhiêu.

## Code 1
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(false); cin.tie(0);
	int c, n;
	cin >> c >> n;
	int a[n];
	for(auto &i: a) cin >> i;
	int res=0;
	function<void(int,int)> f=[&](int x, int y){
		if(x==n){
			if(y<=c) res=max(res, y);
			return ;
		}
		f(x+1, y);
		f(x+1, y+a[x]);
	};
	f(0, 0);
	cout << res;
}
```

## Code 2
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(false); cin.tie(0);
	int c, n;
	cin >> c >> n;
	int a[n];
	for(auto &i: a) cin >> i;
	function<int(int,int)> f=[&](int x, int y){
		if(x==n) return y>c? 0: y;
		return max(f(x+1, y), f(x+1, y+a[x]));
	};
	cout << f(0, 0);
}
```
