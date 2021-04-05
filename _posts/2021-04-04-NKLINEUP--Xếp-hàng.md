---
layout: post
title: NKLINEUP - Xếp hàng
subtitle: vnspoj
tags: [range queries, rmq, max, min, sparse table]
---
Hàng ngày khi lấy sữa, N con bò của bác John (1 ≤ N ≤ 50000) luôn xếp hàng theo thứ tự không đổi. Một hôm bác John quyết định tổ chức một trò chơi cho một số con bò. Để đơn giản, bác John sẽ chọn ra một đoạn liên tiếp các con bò để tham dự trò chơi. Tuy nhiên để trò chơi diễn ra vui vẻ, các con bò phải không quá chênh lệch về chiều cao.

Bác John đã chuẩn bị một danh sách gồm Q (1 ≤ Q ≤ 200000) đoạn các con bò và chiều cao của chúng (trong phạm vi [1, 1000000]). Với mỗi đoạn, bác John muốn xác định chênh lệch chiều cao giữa con bò thấp nhất và cao nhất. Bạn hãy giúp bác John thực hiện công việc này!

## Lời giải
Ta có thể dùng ctdl segment tree hoặc là thuật toán bảng thưa thớt(sparse table algorithm)
- Do các truy vấn trong bài là truy vấn tĩnh nên ta dùng bảng thưa thớt là được rồi, đpt sẽ là **O(nlogn+m)**
- Đpt khi dùng segment tree sẽ là **O((n+m)logn)**

## Code 1

```cpp
#include <bits/stdc++.h>
using namespace std;
int minn(int a, int b){
	return (a<b? a: b);
}
int maxx(int a, int b){
	return (a>b? a: b);
}
struct segment_tree{
	int n, init;
	vector<int> v;
	function<int(int,int)> change;
	segment_tree(int x, int z, function<int(int,int)> y){
		n=x;
		v.assign(2*n, 0);
		change=y;
		init=z;
	}
	void update(int p, int val){
		p+=n;
		v[p]=val;
		for(p/=2; p>0; p/=2){
			v[p]=change(v[p*2], v[p*2+1]);
		}
	}
	int get(int l, int r){
		int res=init;
		l+=n, r+=n;
		while(l<=r){
			if(l%2==1) res=change(res, v[l++]);
			if(r%2==0) res=change(res, v[r--]);
			l/=2, r/=2;
		}
		return res;
	}
};
int main(){
	ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	int n, q;
	cin >> n >> q;
	segment_tree aa(n, 0, maxx);
	segment_tree ii(n,1e9, minn);
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

## Code 3: Sử dụng fenwick tree
fenwick tree có thể sử dụng khi mà ko có thao tác cập nhật giá trị mới nhỏ(lớn) hơn giá trị cũ trong trường hợp fenwick max(min). Cho nên trong TH này có thể dùng fenwick 
Sau đây là code được lấy từ nguồn [vietcodes](https://vietcodes.github.io/code/84/index.html)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void minimize(int &a, int b) {
    if (a > b) a = b;
}
void maximize(int &a, int b) {
    if (a < b) a = b;
}
typedef void (*func)(int &, int);

struct Fenwick {
    int n, init;
    vector<int> f, a;
    func update;

    Fenwick(int n, int val, func func):
        n(n), init(val),
        f(n+1, val), a(n+1),
        update(func) {}

    int get(int l, int r) {
        int result = init;
        while (l <= r) {
            if (r-(r&-r) >= l) update(result, f[r]), r -= r&-r;
            else update(result, a[r]), r -= 1;
        }
        return result;
    }
    void set(int i, int x) {
        a[i] = x;
        for (; i<=n; i += i&-i) update(f[i], x);
    }
};

int main() {
    ios::sync_with_stdio(false); cin.tie(0);

    int n, q; cin >> n >> q;

    Fenwick fmax(n, 0, maximize);
    Fenwick fmin(n, 1e8, minimize);

    for (int i=1; i<=n; i++) {
        int x; cin >> x;
        fmax.set(i, x);
        fmin.set(i, x);
    }

    while (q--) {
        int l, r; cin >> l >> r;
        cout << fmax.get(l, r) - fmin.get(l, r) << endl;
    }

    return 0;
}
```
