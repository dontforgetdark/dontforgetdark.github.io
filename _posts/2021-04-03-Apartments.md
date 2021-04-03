---
layout: post
title: Apartments
subtitle: CSES
tags: [sort, greedy]
---
Có n người và m căn hộ. Mỗi người i muốn mua một căn hộ giá a[i]. Mỗi căn hộ i có giá b[i]. Người mua sẽ chấp nhận mua một căn nhà khi chênh lệch `|b[i]-a[i]|<=k`.

Tính số người tối đa có thể sở hữu cho mình một căn hộ.

## Lời giải
- Thay vì người chọn nhà, ta sẽ đơn giản bài toán thành nhà chọn người
- Để tiện cho việc biện luận ta sort 2 mảng lại.
- Tiếp theo ta quan sát cách đánh giá trong vòng while

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	int n, m, k;
	cin >> n >> m >> k;
	int a[n];
	for(int i=0; i<n; i++) cin >>  a[i];
	sort(a, a+n);
	int b[m];
	for(int i=0; i<m; i++) cin >>  b[i];
	sort(b, b+m);
	int u=0, v=0, res=0;
	while(v<m){
		if(b[v]+k>=a[u] && b[v]-k<=a[u]) u++, v++, res++;
		else if(b[v]-k>a[u]) u++;
		else v++; //b[v]+k<a[u]
	}
	cout << res;
}
```
