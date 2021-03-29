---
layout: post
title:  Queries on Static Arrays
tags: [rmq, prefix, range queries]
---
**Table of content**

- [Sum queries](#sum-queries)
- [Min queries](#min-queries)

## Sum queries
**Bài toán**: Cho mảng a có n phần tử. Q truy vấn, mối truy vấn tính tổng các phần tử đoạn [a, b].

**Thuật toán**: Sử dụng *prefix sum array*
- Tiền xử lý: **O(n)**
- Mỗi truy vấn: **O(1)**

```
sum(a, b) = sum(0, b) - sum(0, a-1)
          = pre[b] - pre[a-1]
```

*Mở rộng: Cho mảng 2 chiều*
![sum2d](assets/img/sum2d.png)
```
S(A) - S(B) - S(C) + S(D)
```
## Min queries
**Bài toán:** Cho mảng a có n phần tử. Q truy vấn, mỗi truy vấn tìm min[^1] của các phần tử đoạn [a, b].

**Thuật toán:** Sử dụng *sparce table Algoithm*
- Tiền xử lý: **O(nlogn)**
- Mỗi truy vấn: **O(1)**

```
min(a, b) = min(min(a, k), min(h, b))
Trong đó: k-a+1, b-h+1 là lũy thừa của 2
```
Ta sẽ tính trước tất cả min của các đoạn có kích thước là lũy thừa của 2. 

Gọi f[i][j] là min của các phần tử đoạn [j, j+(1<<i)-1]
```
int f[LOGN+1][N];
for(int i=0; i<N; i++) f[0][i]=a[i];
for(int i=1; i<=LOGN; i++)
	for(int j=0; j+(1<<i)-1<N; j++)
		f[i][j]=min(f[i-1][j], f[i-1][j+(1<<(i-1))]);
```
Để đảm bảo mỗi truy vấn trong O(1) ta cần tính trước lũy thừa của 2.
```
int log[N+1];
log[1]=1;
for(int i=2; i<=N; i++) log[i]=log[i/2]+1;
```
Mỗi truy vấn ta sẽ lấy min của 2 đoạn là lũy thừa của 2 bao phủ đoạn [a, b].
```
int z=log[b-a+1];	
cout << min(f[z][a], f[z][b-(1<<z)+1]);
```
*Chú thích*

[^1]: Có thể làm tương tự với max.

