---
title: 新老生预选赛题目选讲
date: 2022-10-16 22:07 +0800
categories: [讲义]
tags: [学习]
pin: true
author: Dueyin

toc: true
comments: true

math: false
mermaid: true

typora-root-url: ./..
---

## 新老生预选赛题目选讲

​	emmm，10月18号了，新生也快要入学了，但是新生们好像并不是很勤勉，希望大家加油啊，要入队了。

## 第一道题 E5

#### 题目描述

有一个六位数，其个位数字7，现将个位数字移至首位（十万位），而其余各位数字顺序不变，均后退一位，得到一个新的六位数，假如新数为旧数的4倍，求原来的六位数。

#### 题目分析

这是道填空题啊，打蓝桥杯的时候会遇到，像这种题我们有两种解法，如果是人力可以计算或者能一眼看出来的话就手搓结果就可以了，但如果我们一时想不出啥妙计，就使用程序吧，通过暴力计算，只要能等，程序会给你答案。

比如本题，结果是新数为旧数的四倍，所以我们遍历所有新数比旧数即可，可以发现，新数与旧数是数对，通过两个数的关系，我们发现遍历某个范围的数并将其改造成新数与旧数即可。本题可知旧数个位为'7'，新数十万位为'7'，剩下的部分相同，所以建立i通过遍历0~99999，通过公式710000+i/i*10+100007即可得到答案

```c++
#include<iostream>
using namespace std;
 
int main(){
	int a=710000;
	int b=100007;
	for(int i=0;i<90000;i++){
		int k=a+i;
		int l=b+i*10;
		if(k==4*l){
			cout<<l;
			break;
		}
	}
	return 0;
}
```

## 第二道题P1045

#### 题目描述

有一楼梯共 *n* 级，若每次只能跨上一级或者二级，要走上 *n* 级，共有多少不同走法？ 给定一个正整数 *n* ，请返回一个数，代表上楼的方式数。

数据保证 *n* 小于等于 70。

#### 题目分析

相当经典的一道题，应该学习算法的人都会遇到，可以发现*n*小于70,说明这道题可以用一些时间复杂度比较高的方法，比如——递归。其实这道题通过观察有更简单的理解，甚至一般人都会先发现更简单的方法，但本着科学严谨的治学态度，我们还是讨论一下关于递归的做法。

递归，总的来讲就是分治的思想，本题的思路：可以走一步，也可以走两步，从第n-1阶再走一步能到第n阶，从第n-2阶一次走两步能到第n阶。 所以，f(n)=f(n-1)+f(n-2)。f(n)已经得到了，所以现在要求的就是f(n-1),f(n-2),这样下去，就能通过已知的f(1),f(2)得出结果，显然，大量的重复运算使得这个方法的时间复杂度很高，但有很多题时间复杂度要求并不高，所以递归也是有一定的市场。

```c++
#include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include <algorithm>
using namespace std;
typedef  long long  ll;
int jc(ll x);
int main()
{
    ll n,m;
    cin>>n;
    cout<<(jc(n));
}
int jc(ll x)
{
    if(x==0||x==1)   return 1;//边界很重要
    else  return  jc(x-1)+jc(x-2);
}
```

不过70对于递归来讲似乎都有些大，本题依然有两个样例不能过，那么接下来就是更显而易见的方法，递推。对于递推这个我的理解可能更多的用在发现一定规律的时候，事实上，本题就非常合适，用这个方法，数据规模可以非常高。通过规律，我们发现f(3)=f(1)+f(2),f(4)=f(3)+f(2)...这样就能得出结果，本题和斐波那契数列非常相像，那么下面是代码

```c++
#include<stdio.h>
int main(){
	int n;
	long long int ans;
	scanf("%d",&n);
	long long int a=1;
	long long int b=2;
	long long int sum=2;
	while(sum<n){
		sum++;
		ans=a+b;
		a=b;
		b=ans;
	}
	if(n==1||n==2)printf("%d",n); 
	else printf("%lld",ans);
	return  0;
}
```

## 第三道题P1062

#### 题目描述

现在要求输入 22 个正整数数 *a* , *b* 输出 *a*+*b* 的结果

1<=*a*,*b* 的位数 <= 500

保证输入的数字开头不为 0

#### 题目分析

~~***多么简单的题，c语言一开始就会了！***~~显然，看数据范围，本题没有这么简单，这就是传说中的高精加！？但不过不用紧张，只需要记住这三行代码，就可以解决大部分大数相加的问题（ps：感谢@Alberto提供的技术支持）

```c++
c[i]+=a[i]+b[i];
c[i+1]=c[i]/10;
c[i]=c[i]%10;
```

分析上面的代码，我们发现，用数组来代表每一位数字，是相当不错的选择，剩下的就是模拟加法的过程了。可能这题还涉及到一些字符串类型转换之类的，就不过多赘述了，~~***都在码里***~~

```c++
#include<iostream>
#include<cstring>
using namespace std;

string A,B;
int a[507],b[507],c[507];

int main(){
    cin>>A>>B;
    int n=A.size();
    int m=B.size();
    for(int i=n-1,j=0;i>=0;i--){
        a[j]=A[i]-'0';
    	j++;
    }
    for(int i=m-1,j=0;i>=0;i--){
        b[j]=B[i]-'0';
        j++;
    }
    n=max(n,m);
    for(int i=0;i<n;i++){
		c[i]+=a[i]+b[i];
        c[i+1]=c[i]/10;
        c[i]=c[i]%10;
    }
    if(c[n]!=0)cout<<c[n];
    for(int i=n-1;i>=0;i--)cout<<c[i];
    return 0;
}
```

## 第四题P1102

#### 题目描述

将 3 分解成两个正整数的和，有两种分解方法，分别是 3 = 1 + 2 和3 = 2 + 1。注意顺序不同算不同的方法。将 5 分解成三个正整数的和，有 6 种分解方法，它们是 1+1+3 = 1+2+2 =1 + 3 + 1 = 2 + 1 + 2 = 2 + 2 + 1 = 3 + 1 + 1。请问，将 2021 分解成五个正整数的和，有多少种分解方法？

#### 题目分析

又是一道填空题，所以第一时间想到的就是暴力求解即可。将2021分成五个正整数的和，一开始可能想到五个for循环，事实上也可以这么做，但确实是太慢了。通过观察，我们发现，在选出三个数后，剩下的两个数将会以数对的形式，相加等于2021减去前面三个数，所以，有多少组数对，就代表这三个数下有多少种情况，那么本题就可以缩减到三个for循环即可

```c++
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define mod 1000000009
#define endl "\n"
#define PII pair<int,int>
ll ksm(ll a,ll b) {
	ll ans = 1;
	for(;b;b>>=1LL) {
		if(b & 1) ans = ans * a % mod;
		a = a * a % mod;
	}
	return ans;
}

ll lowbit(ll x){return -x & x;}

const int N = 2e6+10;
int n,a[N];

int main()
{
//	cout<<691677274345<<endl;
//	return 0;
	ll ans = 1;
	for(ll i = 2020,j = 1;i >= 2017; --i,++j) {
		ans *= i;
		ans /= j;
	}
	cout<<ans<<endl;
	
	return 0;
}
//ans = 691677274345

```

PS：谢老师的代码当然更加优秀，所以贴的是他的代码，~~***绝不是因为我懒***~~，另外附上谢老师讲解视频链接https://www.bilibili.com/video/bv11L4y1b7JQ

## 第五题P1037

#### 题目描述

第二届河南省最美教师评选开始了，每一位同学都可以投票选出你支持的人选，但是为了防止刷票，必须通过身份验证才可投票。负责投票平台后台的老大爷希望你能帮他验证身份证号的合法性，防止那些熊孩子随意刷票，下面给出验证规则： (身份证末尾的大写X表示罗马数字10) 采用了ISO 7064:1983.MOD 11-2校验码，以防止不小心记错某一位

##### plain

1、将前面的身份证号码17位数分别乘以不同的系数。从第一位到第十七位的系数分别为：7－9－10－5－8－4－2－1－6－3－7－9－10－5－8－4－2。 2、将这17位数字和系数相乘的结果相加。 3、用加出来和除以11。 4、余数只可能有0－1－2－3－4－5－6－7－8－9－10这11个数字。其分别对应的最后一位身份证的号码为1－0－X －9－8－7－6－5－4－3－2。 5、通过上面得知如果余数是3，就会在身份证的第18位数字上出现的是9。如果对应的数字是2，身份证的最后一位号码就是X。

##### 特别注意：

   "Ⅹ"   是 罗马数字 10， 不是 英文大写字母 "X"， 此处为了编码方便，使用了英文字母 "X" 代替；

现在将给你提供一组身份证号码，请判断哪些是合法的，哪些是不合法的。

#### 题目分析

这题应该算是一道基础题，但想起当初还是给我带来不小的困扰，所以还是挑出来讲了。其实谜底就在谜面上，按照上面的步骤模拟即可，可能需要注意的就是一些写代码时技巧性的东西了

```c++
#include <iostream>
using namespace std;
int main(){
    int T;
    cin >> T;
    while(T--){
        char num[20];
        cin >> num;
        if(num[17] == 'X') 
            num[17] = '9' + 1;
        int cs[] = {7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2};
        int res[] = {1,0,10,9,8,7,6,5,4,3,2};
        int sum = 0;
        for(int i = 0; i < 17; i++){
            sum += (num[i] - '0') * cs[i];
        }
        sum %= 11;
        if(res[sum] + '0' == num[17])
            cout << "True" << endl;
        else 
            cout << "False" << endl;
    }
    return 0;
}
```

## 第六题P1087

#### 题目描述

设有 *n* 个活动的集合 *E*={1,2,..,*n*}，其中每个活动都要求使用同一资源，如演讲会场等，而在同一时间内只有一个活动能使用这一资源。每个活动 *i* 都有一个要求使用该资源的起始时间 *s*i 和一个结束时间 *f*i，且 *s*i<*f*i。如果选择了活动 *i* ，则它在时间区间[*s*i,*f*i) 内占用资源。若区间 [*s*i,*f*i)与区间 [*s*j,*f*j) 不相交，则称活动 *i* 与活动 *j* 是相容的。也就是说，当 f*i≤*s*j 或 *f*j≤*s*i*时，活动 i* 与活动 *j* 相容。选择出由互相兼容的活动组成的最大集合。

#### 题目分析

经典贪心思想的题目，按贪心思想，我们总是留下更多的时间范围来挑选活动，比如[4，7)和[0,5)两个活动，比起时间更短的[4,7)我们更愿意选则[0,5),因为[0,5)结束的更早，也就为之后的活动安排留下更多时间，所以我们总是优先选择最早结束的活动，这就是贪心思想，通过局部最优解，来达到全局最优。在很多题中，我们并不能很明显的考虑到局部最优可以推到全局，但在没有更好的方法时，不妨尝试一下贪心

```c++
#include<bits/stdc++.h>
using namespace std;
struct sum{
	int a,b;
}suum[1005]; 
bool cmp(sum x,sum y){
	return x.b<y.b;
}
int main()
{
	int n,cnt=1,i=0,j=0;
	scanf("%d",&n);
	for(int i=0;i<n;i++)
	{
		scanf("%d %d",&suum[i].a,&suum[i].b);
	}
	sort(suum,suum+n,cmp);
	for(i=1;i<n;i++)
	{
		if(suum[i].a>=suum[j].b)
		{
			j=i;
			cnt++;
		}
	}
	printf("%d",cnt);
	return 0;
}
```

## 第七题E895

#### 题目描述

高考结束了，同学们要开始了紧张的填写志愿的过程，大家希望找一个自己最满意的大学填报方案，请你编程帮忙实现。
现有m(m≤100000)所学校，每所学校预计分数线是ai(ai≤106)。有 n(n≤100000)位学生，估分分别为 bi(bi≤106)。
根据n位学生的估分情况，分别给每位学生推荐一所学校，要求学校的预计分数线和学生的估分相差最小（可高可低，毕竟是估分嘛），这个最小值为不满意度。求所有学生不满意度和的最小值。

#### 题目分析

这题首先想到的是暴力，直接一个一个找，但除非是入门基础语法级选手，我们不得不思考暴力时间复杂度上的缺憾，所以我们需要另寻方案。而熟话说***二分是更高效的暴力***（~~其实是我说的~~），所以，我们理所应当考虑到了二分，事实上，本题也确实是二分。下面是我惯用的一套二分模板

```c++
while(l<=r){
    mid=(l+r)>>1;
    if(check(mid))r=mid-1;
    else l=mid+1;
}
```

二分就是这样一个工具，他可以通过**check**很高效的结合到任何算法中，化腐朽为神奇，所以很多题没有思路都可以考虑一下二分的可能

本题并没有什么弯弯绕绕，二分基本就可以了,但作为初学者，二分过程中的排序当然也是要自己写，才显得够勤勉，所以我们再写一个快速排序，下面是我的快排模板

```c++
void quick_sort(int l,int r){
	if(l>=r) return ;
	int i=l-1,j=r+1,x=a[(l+r+1)/2];
	while(i<j){
		do i++;while(a[i]<x);
		do j--;while(a[j]>x);
		if(i<j){
			int t=a[i];
			a[i]=a[j];
			a[j]=t;
		}
	}
	quick_sort(l,i-1);
	quick_sort(i,r);
}
```

那么要素已经集齐了，下面就是AC代码环节

```c++
#include<iostream>
#include<cmath>
using namespace std;
const int N=1e5+7;

int n,m,i;
int a[N],b[N];

void quick_sort(int l,int r){
	if(l>=r) return ;
	int i=l-1,j=r+1,x=a[(l+r+1)/2];
	while(i<j){
		do i++;while(a[i]<x);
		do j--;while(a[j]>x);
		if(i<j){
			int t=a[i];
			a[i]=a[j];
			a[j]=t;
		}
	}
	quick_sort(l,i-1);
	quick_sort(i,r);
}

bool check(int x){
    if(a[x]>b[i])return true;
    else return false;
}

int erfen(int l,int r){
    while(l<=r){
        int mid=(l+r)>>1;
        if(check(mid))r=mid-1;
    	else l=mid+1;
    }
    return r+1;
}

int main(){
    cin>>m>>n;
    for(int j=1;j<=m;j++)cin>>a[j];
    for(int j=1;j<=n;j++)cin>>b[j];
    quick_sort(1,m);
    int ans=0;
    a[0]=-9999999;
    for(i=1;i<=n;i++){
        int l=erfen(1,m);
        ans+=min(abs(a[l]-b[i]),min(abs(a[l-1]-b[i]),abs(a[l+1]-b[i])));//这个比较是有必要的，因为本题二分能找到的位置只是包括精确位置在内，旁边一个范围里的位置
    }
    cout<<ans;
    return 0;
}

```

## 第八题P1028

#### 题目描述

Krik晚上失眠，躺着很无聊，于是突发奇想道阳台上去看星星。不失所望，那夜繁星当空、星河奔流。Kirk越看越精神，在繁星中发现了一个又一个的数字，刚学编程不久的Kirk想到，电脑上也有星星，何不操作一波呢！

经过一番痛苦的敲键盘事件之后，终于Kirk电脑屏幕上出现了下图十个数字的星星表达式：

![image.png](http://www.mangata.ltd/file/9/A+B%E7%B9%81%E6%98%9F.png)

（本来想给文本的，但是文本太丑了）

为了让大家体会一下Kirk痛苦的敲代码事件，所以Kirk给你两个整数 A 和 B ，你的任务是计算 A+B 的结果并且使用星星表达式输出结果。

#### 题目分析

我最喜欢做这种很好看的题了，不过这种题简单但代码一般都很麻烦，比如这道题，我们必须对每个数字进行改编的函数，相当麻烦，但就和前面说的一样，题本身并不困难,只需要一排一排打每个数字就行了，还有一些大佬操作，我一起贴在下面，有兴趣可以看一下（代码来自Kirk题解，~~***话说这种有题解的干嘛不看题解***~~***）***

***Kirk佬的代码，很好理解***

```c++
#include <stdio.h>

void cout0(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("* *");
    else if (line == 3)
        printf("* *");
    else if (line == 4)
        printf("* *");
    else if (line == 5)
        printf("***");
}

void cout1(int line) {
    if (line == 1)
        printf(" * ");
    else if (line == 2)
        printf(" * ");
    else if (line == 3)
        printf(" * ");
    else if (line == 4)
        printf(" * ");
    else if (line == 5)
        printf(" * ");
}

void cout2(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("  *");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("*  ");
    else if (line == 5)
        printf("***");
}

void cout3(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("  *");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("  *");
    else if (line == 5)
        printf("***");
}

void cout4(int line) {
    if (line == 1)
        printf("* *");
    else if (line == 2)
        printf("* *");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("  *");
    else if (line == 5)
        printf("  *");
}

void cout5(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("*  ");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("  *");
    else if (line == 5)
        printf("***");
}

void cout6(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("*  ");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("* *");
    else if (line == 5)
        printf("***");
}

void cout7(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("  *");
    else if (line == 3)
        printf("  *");
    else if (line == 4)
        printf("  *");
    else if (line == 5)
        printf("  *");
}

void cout8(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("* *");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("* *");
    else if (line == 5)
        printf("***");
}

void cout9(int line) {
    if (line == 1)
        printf("***");
    else if (line == 2)
        printf("* *");
    else if (line == 3)
        printf("***");
    else if (line == 4)
        printf("  *");
    else if (line == 5)
        printf("***");
}

int main() {
    int A, B;
    scanf("%d%d", &A, &B);
    int res = A + B; // 数据保证 10 <= A + B < 100
    int num1 = res / 10;
    int num2 = res % 10;
    for (int j = 1; j <= 6; j++) {
        if (num1 == 0)
            cout0(j);
        else if (num1 == 1)
            cout1(j);
        else if (num1 == 2)
            cout2(j);
        else if (num1 == 3)
            cout3(j);
        else if (num1 == 4)
            cout4(j);
        else if (num1 == 5)
            cout5(j);
        else if (num1 == 6)
            cout6(j);
        else if (num1 == 7)
            cout7(j);
        else if (num1 == 8)
            cout8(j);
        else if (num1 == 9)
            cout9(j);
        putchar(' ');
        if (num2 == 0)
            cout0(j);
        else if (num2 == 1)
            cout1(j);
        else if (num2 == 2)
            cout2(j);
        else if (num2 == 3)
            cout3(j);
        else if (num2 == 4)
            cout4(j);
        else if (num2 == 5)
            cout5(j);
        else if (num2 == 6)
            cout6(j);
        else if (num2 == 7)
            cout7(j);
        else if (num2 == 8)
            cout8(j);
        else if (num2 == 9)
            cout9(j);
        putchar('\n');
    }
    return 0;
}

```

***未知大佬的代码1***

```c++
#include<iostream>
using namespace std;
string x[15][10];
int ans[105],cou;
int main(){
	ios::sync_with_stdio(false);
	x[0][0]="***";
	x[0][1]="* *";
	x[0][2]="* *";
	x[0][3]="* *";
	x[0][4]="***";
	
	x[1][0]=" * ";
	x[1][1]=" * ";
	x[1][2]=" * ";
	x[1][3]=" * ";
	x[1][4]=" * ";
	
	x[2][0]="***";
	x[2][1]="  *";
	x[2][2]="***";
	x[2][3]="*  ";
	x[2][4]="***";
	
	x[3][0]="***";
	x[3][1]="  *";
	x[3][2]="***";
	x[3][3]="  *";
	x[3][4]="***";
	
	x[4][0]="* *";
	x[4][1]="* *";
	x[4][2]="***";
	x[4][3]="  *";
	x[4][4]="  *";
	
	x[5][0]="***";
	x[5][1]="*  ";
	x[5][2]="***";
	x[5][3]="  *";
	x[5][4]="***";
	
	x[6][0]="***";
	x[6][1]="*  ";
	x[6][2]="***";
	x[6][3]="* *";
	x[6][4]="***";
	
	x[7][0]="***";
	x[7][1]="  *";
	x[7][2]="  *";
	x[7][3]="  *";
	x[7][4]="  *";
	
	x[8][0]="***";
	x[8][1]="* *";
	x[8][2]="***";
	x[8][3]="* *";
	x[8][4]="***";
	
	x[9][0]="***";
	x[9][1]="* *";
	x[9][2]="***";
	x[9][3]="  *";
	x[9][4]="***";
	
	int a,b;
	cin>>a>>b;
	int xx=a+b,y=0,yy;
	while(xx){
		ans[cou++]=xx%10;
		xx/=10;
	}
	for(int i=0;i<5;i++){
		yy=y;
		for(int j=cou-1;j>=0;j--){
			cout<<x[ans[j]][i];
			if(j)cout<<' ';
		}
		cout<<endl;
	}
	
	
	
	return 0;
}

#include<iostream>
using namespace std;
char n0[6][6]={"***","* *","* *","* *","***"},n1[6][6]={" * "," * "," * "," * "," * "};
char n2[6][6]={"***","  *","***","*  ","***"},n3[6][6]={"***","  *","***","  *","***"};
char n4[6][6]={"* *","* *","***","  *","  *"},n5[6][6]={"***","*  ","***","  *","***"};
char n6[6][6]={"***","*  ","***","* *","***"},n7[6][6]={"***","  *","  *","  *","  *"};
char n8[6][6]={"***","* *","***","* *","***"},n9[6][6]={"***","* *","***","  *","***"};
int main()
{
	int a,b;
	cin>>a>>b;
	int c=a+b;
	char s[3];

	s[1]=c/10;
	s[2]=c%10;
	int flag=0;
			for(int p=0;p<5;p++)
			{
				for(int j=1;j<3;j++)
				{
					if(s[j]==1)cout<<n1[p];
					if(s[j]==2)cout<<n2[p];
					if(s[j]==3)cout<<n3[p];
					if(s[j]==4)cout<<n4[p];
					if(s[j]==5)cout<<n5[p];
					if(s[j]==6)cout<<n6[p];
					if(s[j]==7)cout<<n7[p];
					if(s[j]==8)cout<<n8[p];
					if(s[j]==9)cout<<n9[p];
					if(s[j]==0)cout<<n0[p];
                    if(j==1)cout<<" ";
				}
				cout<<"\n";
	}
	return 0;
}

```

***未知大佬的代码2***

```c++
#include<iostream>
using namespace std;
char n0[6][6]={"***","* *","* *","* *","***"},n1[6][6]={" * "," * "," * "," * "," * "};
char n2[6][6]={"***","  *","***","*  ","***"},n3[6][6]={"***","  *","***","  *","***"};
char n4[6][6]={"* *","* *","***","  *","  *"},n5[6][6]={"***","*  ","***","  *","***"};
char n6[6][6]={"***","*  ","***","* *","***"},n7[6][6]={"***","  *","  *","  *","  *"};
char n8[6][6]={"***","* *","***","* *","***"},n9[6][6]={"***","* *","***","  *","***"};
int main()
{
	int a,b;
	cin>>a>>b;
	int c=a+b;
	char s[3];

	s[1]=c/10;
	s[2]=c%10;
	int flag=0;
			for(int p=0;p<5;p++)
			{
				for(int j=1;j<3;j++)
				{
					if(s[j]==1)cout<<n1[p];
					if(s[j]==2)cout<<n2[p];
					if(s[j]==3)cout<<n3[p];
					if(s[j]==4)cout<<n4[p];
					if(s[j]==5)cout<<n5[p];
					if(s[j]==6)cout<<n6[p];
					if(s[j]==7)cout<<n7[p];
					if(s[j]==8)cout<<n8[p];
					if(s[j]==9)cout<<n9[p];
					if(s[j]==0)cout<<n0[p];
                    if(j==1)cout<<" ";
				}
				cout<<"\n";
	}
	return 0;
}


```

***本蒟蒻的代码***

```c++
#include<iostream>
using namespace std;

void cou(int x,int line){
    if(line==1){
		if(x==0||x==2||x==3||x==5||x==6||x==7||x==8||x==9)cout<<"***";
        if(x==1)cout<<" * ";
        if(x==4)cout<<"* *";
    }
    if(line==2){
        if(x==0||x==4||x==8||x==9)cout<<"* *";
        if(x==1)cout<<" * ";
        if(x==2||x==3||x==7)cout<<"  *";
        if(x==5||x==6)cout<<"*  ";
    }
    if(line==3){
		if(x==0)cout<<"* *";
        if(x==1)cout<<" * ";
        if(x==2||x==3||x==4||x==5||x==6||x==8||x==9)cout<<"***";
        if(x==7)cout<<"  *";
    }
    if(line==4){
		if(x==0||x==6||x==8)cout<<"* *";
        if(x==1)cout<<" * ";
        if(x==2)cout<<"*  ";
        if(x==3||x==4||x==5||x==7||x==9)cout<<"  *";
    }
    if(line==5){
        if(x==0||x==2||x==3||x==5||x==6||x==8||x==9)cout<<"***";
        if(x==1)cout<<" * ";
        if(x==4||x==7)cout<<"  *";
    }
}

int main(){
    int a,b;
    cin>>a>>b;
    int c=a+b;
    b=c%10;
    a=(c-b)/10;
    for(int i=1;i<=5;i++){
        cou(a,i);
        cout<<" ";
        cou(b,i);
        cout<<endl;
    }
    return 0;
}
```

***就是这样，喵~***



## 结语

那么，我的第一次讲题就到此结束了，不足之处还望指正
