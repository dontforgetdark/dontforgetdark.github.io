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
<Spoiler title="Code">

The DSU operations take $\mathcal{O}(\log n)$ rather than
$\mathcal{O}(\alpha(n))$ because the DSU does not use union by size, but it's
easy to change this.

```cpp
struct TwoEdgeCC {
	struct {
		vi e; void init(int n) { e = vi(n,-1); }
		int get(int x) { return e[x] < 0 ? x : e[x] = get(e[x]); }
		bool unite(int x, int y) { // set par[y] = x
			x = get(x), y = get(y); if (x == y) return 0;
			e[x] += e[y]; e[y] = x; return 1;
		}
	} DSU;
	int N; vector<vi> adj; vi depth, par;
	vpi extra;
	void init(int _N) {
		N = _N; DSU.init(N);
		adj.rsz(N), depth.rsz(N), par = vi(N,-1);
	}
	void dfs(int x) {
		trav(t,adj[x]) if (t != par[x])
			par[t] = x, depth[t] = depth[x]+1, dfs(t);
	}
	void ae(int a, int b) {
		if (DSU.unite(a,b)) adj[a].pb(b), adj[b].pb(a); // edge of forest
		else extra.pb({a,b}); // extra edge
	}
	void ad(int a, int b) {
		while (1) {
			a = DSU.get(a), b = DSU.get(b);
			if (a == b) return;
			if (depth[a] < depth[b]) swap(a,b);
			assert(par[a] != -1 && DSU.unite(par[a],a));
		}
	}
	void gen() {
		F0R(i,N) if (par[i] == -1) dfs(i); // independently for each connected component
		DSU.init(N); trav(t,extra) ad(t.f,t.s); // add non-spanning edges
};
```

</Spoiler>
