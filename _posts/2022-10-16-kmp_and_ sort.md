---

title: 「一本通 1.2 练习 2」扩散（经典题型精析）
date: 2022-10-16 9:48 +0800
categories: [例题]
tags: [学习]
pin: true
author: Duyin

toc: true
comments: true

math: false
mermaid: true

typora-root-url: ./..
---

# 并查集求距离

#### **题目描述**

一个点每过一个单位时间就会向 4 个方向扩散一个距离，如图所示：两个点 *a* 、*b* 连通，记作 *e*(*a*,*b*)，当且仅当 *a* 、*b* 的扩散区域有公共部分。连通块的定义是块内的任意两个点 *u*、*v* 都必定存在路径*e*(*u*, *a*_0 ),*e*(*a*0,*a*_1),…*e*(*a*k,*v*)。

给定平面上的 *n* 个点，问最早什么时候它们形成一个连通块。

![img](https://i.loli.net/2018/07/03/5b3ac26b7a6a4.jpg)

#### 输入格式

第一行一个数 *n* ，以下 *n* 行，每行一个点坐标。

#### 输出格式

输出仅一个数，表示最早的时刻所有点形成连通块。

#### 样例

##### 输入

```input
2
0 0
5 5
```

##### 输出

```output
5
```



#### 数据范围与提示

对于 20% 的数据，满足 1≤*n*≤5,1≤*X*i,*Y*i≤50；

对于 100\%100% 的数据，满足 1≤*n*≤50,1≤*X*i,*Y*i≤10^9。

# 思路分析

本题有两种解法，一是floyd，二是并查集求距离。本题的floyd算法比较常规，经典的是并查集加二分的思路，可以说是非常的巧妙。但是俗话说的好，~~***来都来了***~~，不如就把floyd的思路同样分析一下。首先，传统的floyd讲究求最短路，即n点之间通过收缩的方式找出两两之间的”最短距离“。其实我们不妨把这个思路抽象一下，有n个客体，直接连通两个客体的代价是x，如果通过第三个客体连通，那么代价将减小为y，所以我们更新代价为y。而这道题的客体就是点，代价就是|X1-X2|+|y1-y2|（~~***小学数学，应该都懂吧***~~），所以，~~***可以直接套用floyd***~~。事实上不完全可以，我们发现本题不是简单的收缩，求i，j之间的中间点k不是经过k然后相连，而是谁到k的距离最远，只要把该点与k相连，k就成为这两点在这题中的代理了：

#### floyd

```c++
for(int k=0;k<n;k++){
    for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
            if(j!=i&&j!=k)v[i][j]=min(v[i][j],max(v[i][k],v[k][j]));
        }
    }
}
```

我们需要一个函数来求距离：

#### 距离

```c++
for(int i=0;i<n;i++){
    for(int j=0;j<n;j++){
		if(i!=j){
			v[i][j]=abs(x[i]-x[j])+abs(y[i]-y[j]);
        }
    }
}
```

因为结束时间为最长距离的连接时间，所以最后我们求最长距离即可：

#### 最长距离

```c++
for(int i=0;i<n;i++){
    for(int j=0;j<n;j++){
        ans=max(ans,v[i][j]);
    }
}
```

那么~~***万物皆虚,万事皆允***~~，我们来看看最终代码吧：

## 代码实现1

```c++
#include<iostream>
#include<cmath>
using namespace std;
const int N=57;

int n;
int x[N],y[N];
int v[N][N];

void find_distance(){
	for(int i=0;i<n;i++){
    	for(int j=0;j<n;j++){
			if(i!=j){
				v[i][j]=v[j][i]=abs(x[i]-x[j])+abs(y[i]-y[j]);
  			}
    	}
	}
}

void floyd(){
    for(int k=0;k<n;k++){
    	for(int i=0;i<n;i++){
			for(int j=0;j<n;j++){
            	if(j!=i&&j!=k)v[i][j]=min(v[i][j],max(v[i][k],v[k][j]));
        	}
    	}
	}
}

void find_ans(){
    int ans=0;
    for(int i=0;i<n;i++){
    	for(int j=0;j<n;j++){
    	    ans=max(ans,v[i][j]);
   		}
	}
    cout<<(ans+1)/2;
}

int main(){
	cin>>n;
	for(int i=0;i<n;i++){
		cin>>x[i]>>y[i];
	}
	find_distance();
	floyd();
	find_ans();
	return 0;
}
```

floyd+dp已经结束辣！下面是这道题更为精妙的做法：
并查集+二分！

二分有一种我自己的理解方式是这样的，通过二分来查找某一个答案，来缩减问题的难度，二分本质是更***高效的遍历！！！***例如本题，我们通过二分来遍历最小时间，通过并查集来检查是否连通，如果连通就继续减少时间，如果不连通，就扩大，下面是代码环节：

#### 二分

```c++
while(l<=r){
    mid=(l+r)>>1;
    if(check(mid))r=mid-1;
    else l=mid+1;
}
```

#### 并查集+check

```c++
for(int i=0;i<n;i++)fa[i]=i;
for(int i=0;i<n;i++){
    for(int j=0;j<n;j++){
        if(v[i][j]<=2*x){
            int fx=f(i),fy=f(j);
            if(fx!=fy)fa[fx]=fy;
        }
    }
}
int sum = 0 ;
for(int i=0;i<n;i++){
    if(fa[i]==i)sum++;
    if(sum==2)return false;
}
return true;
```

#### 距离

```c++
for(int i=0;i<n;i++){
    for(int j=0;j<n;j++){
		if(i!=j){
			v[i][j]=abs(x[i]-x[j])+abs(y[i]-y[j]);
        }
    }
}
```

#### 查找祖先

```c++
if(x==fa[x])return x;
else return fa[x]=find(fa[x]);
```

来看看最终代码吧：

## 代码实现2

```c++
#include<iostream>
#include<cmath>
using namespace std;
const int N=57;
int n;
int fa[N];
int v[N][N];
int x[N],y[N];

int f(int x){
    if(x==fa[x])return x;
	else return fa[x]=f(fa[x]);
}

void find_distance(){
    for(int i=0;i<n;i++){
    	for(int j=0;j<n;j++){
			if(i!=j){
				v[i][j]=abs(x[i]-x[j])+abs(y[i]-y[j]);
        	}
    	}
	}
}

bool check(int x){
    for(int i=0;i<n;i++)fa[i]=i;
	for(int i=0;i<n;i++){
    	for(int j=0;j<n;j++){
        	if(v[i][j]<=2*x){
            	int fx=f(i),fy=f(j);
           		if(fx!=fy)fa[fx]=fy;
        	}
    	}
	}
	int sum = 0 ;
	for(int i=0;i<n;i++){
    	if(fa[i]==i)sum++;
    	if(sum==2)return false;
	}
	return true;
}

int erfen(int l,int r){
    int mid;
    while(l<=r){
    	mid=(l+r)>>1;
    	if(check(mid))r=mid-1;
    	else l=mid+1;
	}
    return r+1;
}

int main(){
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>x[i]>>y[i];
    
    find_distance();
    int ans=erfen(0,999999999);
    cout<<ans;
    
    return 0;
}

```

至此，关于本题的分享就到此结束了