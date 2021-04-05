---
layout: post
title: NKLINEUP - Xếp hàng
subtitle: vnspoj
tags: [range queries, rmq, max, min, sparse table]
---
Hàng ngày khi lấy sữa, N con bò của bác John (1 ≤ N ≤ 50000) luôn xếp hàng theo thứ tự không đổi. Một hôm bác John quyết định tổ chức một trò chơi cho một số con bò. Để đơn giản, bác John sẽ chọn ra một đoạn liên tiếp các con bò để tham dự trò chơi. Tuy nhiên để trò chơi diễn ra vui vẻ, các con bò phải không quá chênh lệch về chiều cao.

Bác John đã chuẩn bị một danh sách gồm Q (1 ≤ Q ≤ 200000) đoạn các con bò và chiều cao của chúng (trong phạm vi [1, 1000000]). Với mỗi đoạn, bác John muốn xác định chênh lệch chiều cao giữa con bò thấp nhất và cao nhất. Bạn hãy giúp bác John thực hiện công việc này!

## Lời giải
Ta có thể dùng ctdl segment tree hoặc là thuật toán bảng thưa thớt(sparce table algorithm)
- Do các truy vấn trong bài là truy vấn tĩnh nên ta dùng bảng thưa thớt là được rồi, đpt sẽ là **O(nlogn+m)**
- Đpt khi dùng segment tree sẽ là **O((n+m)logn)**

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
## Code 2
```cpp
#include <bits/stdc++.h>
using namespace std;
#define debug(x) cerr << #x <<  " = " << x <<  endl;
int main(){
	int n, q;
	cin >> n >> q;
	int k=log2(n);
	int a[k+1][n+1];
	int b[k+1][n+1];
	for(int i=1; i<=n; i++){
		cin >> a[0][i];
		b[0][i]=a[0][i];
	}
	for(int i=1; i<=k; i++){
		for(int j=1; j+(1<<i)-1<=n; j++){
			a[i][j]=max(a[i-1][j], a[i-1][j+(1<<(i-1))]);
			b[i][j]=min(b[i-1][j], b[i-1][j+(1<<(i-1))]);
		}
	}
	int lg[n+1];
	lg[1]=0;
	for(int i=2; i<=n; i++) lg[i]=lg[i/2]+1;
	while(q--){
		int l, r;
		cin >> l >> r;
		int x=r-l+1;
		int ma=max(a[lg[x]][l], a[lg[x]][r-(1<<lg[x])+1]);
		int mi=min(b[lg[x]][l], b[lg[x]][r-(1<<lg[x])+1]);
		cout << ma - mi << '\n';
	}
}
```
**Lưu ý:** lỗi sai đã mắc là nhầm lẫn giữa hàm log(x) và log2(x). Theo thứ tự chúng là cơ số e và cơ số 2.
