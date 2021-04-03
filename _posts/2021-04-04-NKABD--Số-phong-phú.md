---
layout: post
title: 
NKABD - Số phong phú
subtitle: vnspoj
tags: [theorynumber, divisor]
---
Trong số học, số phong phú là các số mà tổng các ước số của số đó (không kể chính nó) lớn hơn số đó. Ví dụ, số 12 có tổng các ước số (không kể 12) là 1 + 2 + 3 + 4 + 6 = 16 > 12. Do đó 12 là một số phong phú.

Bạn hãy lập trình đếm xem có bao nhiêu số phong phú trong đoạn [L,R].

## Lời giải
- Tất cả các ước số có thể có sẽ là [1, R/2], với mỗi số i trong đoạn này ta phân bố vào sum[j] với j là các bội số của i.

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	int l, r;
	cin >> l >> r;
	vector<int> sum(r+1, 0);
	for(int i=1; i<=r/2; i++){
		for(int j=(l+i-1)/i*i; j<=r; j+=i){
			if(i==j) continue;
			sum[j]+=i;
		}
	}
	int res=0;
	for(int i=l; i<=r; i++){
		res+=(sum[i]>i);
	}
	cout << res;
}
```
