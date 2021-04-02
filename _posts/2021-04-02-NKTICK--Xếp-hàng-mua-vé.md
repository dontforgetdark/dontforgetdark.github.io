---
layout: post
title: NKTICK - Xếp hàng mua vé
subtitle: vnspoj
tags: [dp]
---
Có N người sắp hàng mua vé dự buổi hoà nhạc. Ta đánh số họ từ 1 đến N theo thứ tự đứng trong hàng. Mỗi người cần mua một vé, song người bán vé được phép bán cho mỗi người tối đa hai vé. Vì thế, một số người có thể rời hàng và nhờ người đứng trước mình mua hộ vé. Biết ti là thời gian cần thiết để người i mua xong vé cho mình. Nếu người i+1 rời khỏi hàng và nhờ người i mua hộ vé thì thời gian để người thứ i mua được vé cho cả hai người là ri.

Yêu cầu: Xác định xem những người nào cần rời khỏi hàng và nhờ người đứng trước mua hộ vé để tổng thời gian phục vụ bán vé là nhỏ nhất.

Dữ liệu
- Dòng đầu tiên chứa số N (1 ≤ N ≤ 60000).
- Dòng thứ 2 ghi N số nguyên dương t1, t2, ..., tN. (1 ≤ ti ≤ 30000)
- Dòng thứ ba ghi N-1 số nguyên dương r1, r2, ..., rN-1. (1 ≤ ri ≤ 30000)
Kết qủa
- In ra tổng thời gian phục vụ nhỏ nhất.

Ví dụ
```
Dữ liệu:
5
2 5 7 8 4
4 9 10 10 

Kết qủa
18

Dữ liệu:
4
5 7 8 4
50 50 50 

Kết qủa
24
```
[submit](https://vn.spoj.com/problems/NKTICK/)

**Lời giải**

Đặt f[x] là số tiền phải bỏ ra ít nhất để các người trong đoạn [x, n) mua vé.

**Code 1**: topdown
```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF=2e9;
int main(){
	int n;
	cin >> n;
	int t[n];
	for(int i=0; i<n; i++) cin >> t[i];
	int r[n];
	for(int i=0; i<n-1; i++) cin >> r[i];
	r[n-1]=t[n-1];
	int dp[n];
	fill(dp, dp+n, INF);
	function<int(int)> f=[&](int x){
		if(x>=n) return 0;
		if(dp[x]!=INF) return dp[x];
		return dp[x]=min(f(x+2)+r[x], f(x+1)+t[x]);
	};
	cout << f(0);
} 
```
**code 2:** bottomup
```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF=2e9;
int main(){
	int n;
	cin >> n;
	int t[n];
	for(int i=0; i<n; i++) cin >> t[i];
	int r[n];
	for(int i=0; i<n-1; i++) cin >> r[i];
 
	int dp[n];
	dp[n-1]=t[n-1];
	if(n>1) dp[n-2]=min(r[n-2], t[n-2]+dp[n-1]);
	for(int i=n-3; i>=0; i--){
		dp[i]=min(dp[i+2]+r[i], dp[i+1]+t[i]);
	}
	cout << dp[0];
} 
```
