---
layout: post
title: NKSGAME - VOI08 Trò chơi với dãy số
subtitle: vnspoj
tags: [binary_search, lower_bound]
---
Cho 2 mảng có n phần tử. Mỗi phần tử chọn ra 1 phần tử sao cho abs(tổng 2 phần tử được chọn) là nhỏ nhất.

## Lời giải
- Sắp xếp các mảng a tăng dần để chặt nhị phân.
- Với mỗi giá trị `val` của mảng b, có 2 giá trị gần với `-val` nhất(1 giá trị `>=` và 1 giá trị `<`)

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
#define debug(x) cerr << #x << " = " << x << endl;
const int INF=2e9;
int main(){
	int n;
	cin >> n;
	int a[n];
	for(int i=0; i<n; i++) cin >> a[i];
	sort(a, a+n);
	int res=INF;
	for(int i=0; i<n; i++){
		int val;
		cin >> val;
		int *x=lower_bound(a, a+n, -val);
		if(x-a<n) res=min(abs(*x+val), res);
		if(x-a>0) res=min(abs(*(--x)+val), res);
	}
	cout << res;
} 
```
