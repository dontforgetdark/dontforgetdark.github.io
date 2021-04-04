---
layout: post
title: NKLINEUP - Xếp hàng
subtitle: vnspoj
tags: [range queries, rmq, max, min, sparce table]
---
Hàng ngày khi lấy sữa, N con bò của bác John (1 ≤ N ≤ 50000) luôn xếp hàng theo thứ tự không đổi. Một hôm bác John quyết định tổ chức một trò chơi cho một số con bò. Để đơn giản, bác John sẽ chọn ra một đoạn liên tiếp các con bò để tham dự trò chơi. Tuy nhiên để trò chơi diễn ra vui vẻ, các con bò phải không quá chênh lệch về chiều cao.

Bác John đã chuẩn bị một danh sách gồm Q (1 ≤ Q ≤ 200000) đoạn các con bò và chiều cao của chúng (trong phạm vi [1, 1000000]). Với mỗi đoạn, bác John muốn xác định chênh lệch chiều cao giữa con bò thấp nhất và cao nhất. Bạn hãy giúp bác John thực hiện công việc này!

## Lời giải
Ta có thể dùng ctdl segment tree hoặc là thuật toán bảng thưa thới(sparce table algorithm)

## Code 1

```cpp
#include <bits/stdc++.h>
using namespace std;
struct segmin{
	int n;
	vector<int> v;
	segmin(int x){
		n=x;
		v.assign(2*n, 1e9);
	}
	void update(int p, int val){
		p+=n;
		v[p]=val;
		for(p/=2; p>0; p/=2){
			v[p]=min(v[p*2], v[p*2+1]);
		}
	}
	int get(int l, int r){
		int res=1e9;
		l+=n, r+=n;
		while(l<=r){
			if(l%2==1) res=min(res, v[l++]);
			if(r%2==0) res=min(res, v[r--]);
			l/=2, r/=2;
		}
		return res;
	}
};
struct segmax{
	int n;
	vector<int> v;
	segmax(int x){
		n=x;
		v.assign(2*n, 0);
	}
	void update(int p, int val){
		p+=n;
		v[p]=val;
		for(p/=2; p>0; p/=2){
			v[p]=max(v[p*2], v[p*2+1]);
		}
	}
	int get(int l, int r){
		int res=0;
		l+=n, r+=n;
		while(l<=r){
			if(l%2==1) res=max(res, v[l++]);
			if(r%2==0) res=max(res, v[r--]);
			l/=2, r/=2;
		}
		return res;
	}
};
int main(){
	int n, q;
	cin >> n >> q;
	segmax aa(n);
	segmin ii(n);
	for(int i=0; i<n; i++){
		int x;
		cin >> x;
		aa.update(i, x);
		ii.update(i, x);
	}
	while(q--){
		int l, r;
		cin >> l >> r;
		l--, r--;
		cout << aa.get(l, r)-ii.get(l, r) << "\n";
	}
}
```
