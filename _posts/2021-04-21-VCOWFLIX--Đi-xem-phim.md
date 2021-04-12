---
layout: post
title: VCOWFLIX - Đi xem phim
subtitle: vnspoj
tags: [recursion, dp, bitmask]
---
Nông dân John đang đưa các con bò của anh ta đi xem phim! Xe tải của anh ta thì có sức chứa có hạn thôi, là C (100≤C≤5000) kg, anh ta muốn đưa 1 số con bò đi xem phim sao cho tổng khối lượng của đống bò này là lớn nhất, đồng thời xe tải của anh ta vẫn chịu được.

Cho N (1≤N≤16) con bò và khối lượng Wi của từng con, hãy cho biết khối lượng bò lớn nhất mà John có thể đưa đi xem phim là bao nhiêu.

## Lời giải

- Ta duyệt mảng từ đầu tới cuối, với mỗi phần tử ta có 2 lựa chọn(1 là chọ, 2 là ko chọn). Như vậy sau cùng khi tới cuối mảng ta sẽ có tất cả các tổng có thể có, đpt của thuật là O(2^n)

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

## Code 2: nâng cấp từ code 1
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
## Code 3: không đệ quy, style sinh tổ hợp
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
	ios::sync_with_stdio(false); cin.tie(0);
	int c, n;
	cin >> c >> n;
	int a[n];
	for(auto &i: a) cin >> i;
	vector<int> v(1, 0);
	int res=0;
	for(auto val: a){
		int sz=v.size();
		for(int i=0; i<sz; i++){
			int x=v[i]+val;
			if(x>c) continue;
			v.push_back(x);
			res=max(res, x);
		}
	}
	cout << res;
}
```
## Code 4: style bitmask 

*(nguồn vietcodes.github.io)*

- Sinh 2^n TH, với mỗi TH sẽ có 1 dãy bit...xét bit cuối xem là 1 ko... rồi dịch bit... rồi lại xét bit cuối...

```cpp
#include <iostream>
#include <vector>
using namespace std;

int sum(int s, const vector<int> &w) {
    int res = 0;
    for (int x: w) {
        if(s&1) res += x;
        s >>= 1;
    }
    return res;
}

int main() {
    int c, n; cin >> c >> n;
    vector<int> w(n);
    for (int &x: w) cin >> x;
    int res = 0;
    for (int i=0; i<1<<n; i++) {
        int weight = sum(i, w);
        if (weight <= c && weight > res) res = weight;
    }
    cout << res;
    return 0;
}
```
