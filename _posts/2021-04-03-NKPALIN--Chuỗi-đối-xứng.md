---
layout: post
title: NKPALIN - Chuỗi đối xứng
subtitle: vnspoj
tags: [dp, string]
---
Một chuỗi được gọi là đối xứng (palindrome) nếu như khi đọc chuỗi này từ phải sang trái cũng thu được chuỗi ban đầu.

Yêu cầu: tìm một chuỗi con đối xứng dài nhất của một chuỗi s cho trước. Chuỗi con là chuỗi thu được khi xóa đi một số ký tự từ chuỗi ban đầu.

## Lời giải
- Gọi f[x][y] là độ dài panlindrome dài nhất có thể từ chuỗi s[x, y]
- Sau đó ta biện luận để tìm mảng dài nhất của chuỗi s

## Code 1: Không đệ quy
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	string s;
	cin >> s;
	int n=s.length();
	vector<vector<int>> dp(n, vector<int>(n, 1));
	for(int l=n-2; l>=0; l--){
		for(int r=1; r<n; r++){
			if(s[l]==s[r]) dp[l][r]=1+(l<r)+dp[l+1][r-1];
			else dp[l][r]=max(dp[l][r-1], dp[l+1][r]);
		}
	}
	string res;
	int l=0, r=n-1;
	while(l<r){
		if(s[l]==s[r]) res.push_back(s[l]), l++, r--;
		else{
			if(dp[l][r-1]>dp[l+1][r]) r--;
			else l++;
		}
	}
	cout << res;
	if(l==r) cout << s[l];
	for(int i=(int)res.size()-1; i>=0; i--){
		cout << res[i];
	}
} 
```
## Code 2: đệ quy
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
	string s;
	cin >> s;
	int n=s.length();
	vector<vector<int>> dp(n, vector<int>(n, -1));
	function<int(int,int)> f=[&](int l, int r){
		if(l>r) return 0;
		if(dp[l][r]!=-1) return dp[l][r];
		if(s[l]==s[r]) return dp[l][r]=1+(l<r)+f(l+1, r-1);
		return dp[l][r]=max(f(l, r-1), f(l+1, r));
	};
	f(0, n-1);
	string res;
	bool odd=0;
	function<void(int,int)> g=[&](int l, int r){
		if(l>r) return ;
		if(s[l]==s[r] && l<r) res.push_back(s[l]), g(l+1, r-1);
		else if(l<r){
			if(dp[l][r]==dp[l+1][r]) g(l+1, r);
			else g(l, r-1);
		}
		else res.push_back(s[l]), odd=1;
	};
	g(0, n-1);
	cout << res;
	for(int i=(int)res.size()-1-odd; i>=0; i--){
		cout << res[i];
	}
} 
```
