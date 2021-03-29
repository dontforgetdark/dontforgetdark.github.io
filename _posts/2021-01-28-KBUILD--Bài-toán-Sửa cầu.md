---
layout: post
title: KBUILD - Bài toán Sửa cầu
subtitle: VNOI
tags: [ds, dsu, tree, dfs and similar, path compression]
---
Cho cây có n đỉnh, n-1 cạnh. m truy vấn, mỗi truy vấn đánh dấu các cạnh đã đi qua trên đường đi từ a đến b. Sau cùng hỏi có bao nhiêu cạnh chưa được đi qua.

[submit](https://codeforces.com/group/FLVn1Sc504/contest/274816/problem/D)

## Lời giải

- Chọn đỉnh 1 là root của cây, dfs(1) để tính độ sâu của các đỉnh trong cây, và tìm cha cho các đỉnh.
  + h[i] là độ sâu của đỉnh i.
  + par[i] là cha của đỉnh i.
- Khởi tạo dsu-path compression có hàm tìm root.
- Với mỗi cặp (u, v) trong m truy vấn, ta **nhảy** cho đến khi chúng có chung root.

## Showcode

```c++
#include <bits/stdc++.h>
using namespace std;
#define debug(x) cerr << #x << " = " << x << endl;
#define int long long
main(){
	ios_base::sync_with_stdio(false); cin.tie(0); cout.tie(0);
	int n;
	cin >> n;
	vector<int> adj[n+1];
	for(int i=1; i<n; i++){
		int u, v;
		cin >> u >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	vector<int> par(n+1, 0);
	vector<int> h(n+1, 0);
	function<void(int)> dfs=[&](int u){
		for(auto v: adj[u]){
			if(v==par[u]) continue;
			h[v]=h[u]+1;
			par[v]=u;
			dfs(v);
		}
	};
	dfs(1);
	vector<int> link(n+1);
	for(int i=1; i<=n; i++) link[i]=i;
	function<int(int)> root=[&](int a){
		return a==link[a]? a: link[a]=root(link[a]);
	};
	int m;
	cin >> m;
	int res=n-1;
	while(m--){
		int u, v;
		cin >> u >> v;
		u=root(u), v=root(v);
		while(u!=v){
			res--;
			if(h[u]>=h[v]) u=link[u]=root(par[u]);
			else v=link[v]=root(par[v]);
		}
	}
	cout << res;
}
```

## Giải thích thêm

- Tại sao không dùng dsu bình thường mà phải dùng thêm path compression?

**Trả lời**: Do dsu bình thường sẽ bị tle. Điểm hay của path compression là trên đường đi tìm root của một đỉnh, nó đặt lại root cho tất cả các đỉnh đã đi qua(xem lại hàm root để hiểu rõ).

- Tại sao vòng while không bị tle?

**Trả lời**: Cách đơn giản là ta dựa vào biến `res`. Ta thấy res ban đâu đại biểu có `n-1` con đường chưa đi, mỗi lần nhảy lên độ sâu nhỏ hơn thì `res--`, tức là đó là con đường chưa đi và bây giờ phải đi, như vậy không thể quá `n-1` được.

- Tại sao là `u=link[u]=root(par[u])` mà không phải `u=link[u]=par[u]`?

**Trả lời**: Vì `par[u]` là cha của `u`, có tác dụng giúp ta nhảy lên độ sâu nhỏ hơn, Còn `root(par[u])` là thằng đại diện của `par[u]`, ta cần nhảy lên thằng này nếu không muốn tle.
