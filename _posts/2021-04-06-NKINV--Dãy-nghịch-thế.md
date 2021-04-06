---
layout: post
title: NKINV - Dãy nghịch thế
subtitle: vnspoj
tags: [fenwick, BIT, segment tree]
---
Cho một dãy số a1.. aN. Một nghịch thế là một cặp số u, v sao cho u < v và au > av. Nhiệm vụ của bạn là đếm số nghịch thế.

## Lời giải
- Với mỗi phần tử ở vị trí i ta tính xem có bao nhiêu phần tử a[x]>a[i] mà x thuộc [1, i), trong bài này chỉ số đánh từ 1 để tiện áp dụng fenwick.

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
struct fenwick{
	int n;
	vector<int> v;
	fenwick(int n): 
	n(n), v(n+1, 0){}
	void update(int p){
		while(p>0){
			v[p]++;
			p-=p&-p;
		}
	}
	int get(int p){
		int res=0;
		while(p<=n){
			res+=v[p];
			p+=p&-p;
		}
		return res;
	}
};
int main(){
	ios::sync_with_stdio(false); cin.tie(0);
	int n;
	cin >> n;
	fenwick fe(60000);
	long long res=0;
	for(int i=1; i<=n; i++){
		int x;
		cin >> x;
		res+=fe.get(x+1);
		fe.update(x);
	}
	cout << res;
}
```
