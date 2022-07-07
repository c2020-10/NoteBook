# FFT(快速傅里叶变换)
多项式$f(X)=a_0X^0+a_1X^1+...+a_nX^n$其中$a_0,a_1,...,a_n$为多项式的系数。

## 前置知识

### 多项式

假设当前有两个多项式$f(X),g(X)$,现在要把他们乘起来，朴素做法是
$\large \sum_{i=0}^{2n-1}(\sum_{j=0}^{i}a_j*b_{i-j})*x^i$
	
### 复数

复数形式$a+bi$，其中$i=\sqrt{-1}$

复数乘法$(a_1+b_1i)*(a_2+b_2i)=(a_1a_2-b_1b_2)+(a_1b_1+a_2b_1)i$

### 单位根

复数$w$满足$w^n1$称作$w$是$n$次单位根

单位根的性质如下
$\large w_{2n}^{2m}=w_n^m$
$\large w_n^m=-w_n^{m+\frac{n}{2}}$

### 多项式的系数表示法

$f(X)=a_0X^0+a_1X^1+...+a_nX^n$

### 多项式的点值表示法

任取$n+1$个互不相同的$\large S={p_1,p_2,...,p_{n+1}}$,对$f(X)$求值得到$f(p_1),f(p_2),...,f(p_{n+1})$,此时称$A(X)={(p_1,f(p_1)),p_2,f(p_2)),...,p_{n+1},f(p_{n+1}))}$为多项式$f(X)$在$S$下的点值表示法

可以把多项式想象成一个$n$次函数，点值表示法就是取$S$下每一个横坐标时对应的点，因为$n$次函数可以由$n + 1$个点确定下来(可以将每一个点列一个$n$次方程)，所以$n$维点值与$n$维系数一一对应

设两个多项式$P,Q$分别取$(x,y_1),(x,y_2)$,$P*Q$就会取到点$(x,y1*y2)$
令$C=P*Q$,因为$C(X)=P(X)*Q(X)$,所以$C(x)=P(X)*Q(X)$,即$C(X)=y_1*y_2$,在点值表示法下，计算多项式的乘法是$\Theta(n)$的

### FFT的具体过程

FFT就是将系数表示法转化成点值表示法相乘，再由点值表示法转化为系数表示法的过程，第一个过程叫做求值(DFT)，第二个过程叫做插值(IDFT)

### 求值

想要求出一个多项式的点值表示法，需要选出$n+1$个数分别带入到多项式里面，带入一个数的复杂度是$\varTheta(n)$的，那么总复杂度就是$\Theta(n^2)$的，因为单位根有上面两个优美的性质，所以我们尝试可以取$n$次单位根组成$S$，看看能不能加速我们的运算

设$A_0(X)$为$A(X)$偶次项的和，设$A_1(X)$为$A(X)$的奇次项的和，即：

$\large A_0(X)=a_0x^0+a_2x^1+...+a_{n-1}x^{n/2}$
$\large A_1(X)=a_1x^0+a_3x^1+...+a_{n-2}x^{n/2}$

因为$\large A(w_n^m)=a_0w_n^0+a_1w_n^m+...+a_nw_n^{nm}$

所以
$\large A(w_n^m)=A_0(w_{\frac{n}{2}}^m)+w_n^mA_1(w_{\frac{n}{2}}^{m})$

$\large A(w_n^{m+\frac{n}{2}})=A_0(w_{\frac{n}{2}}^m)-w_n^mA_1(w_{\frac{n}{2}}^{m})$

## 【模板】A*B Problem 升级版（FFT 快速傅里叶变换）

```cpp
#include<bits/stdc++.h>
#define x first
#define y second
#define int long long 
#define endl '\n'
using namespace std;
typedef pair<int,int>PII;

const int N=10000006;

const double PI=acos(-1);
int n,m;


//复数
struct Complex{
	double x,y;
	Complex operator +(const Complex &t)const {
		return {x+t.x,y+t.y};
	}
	Complex operator -(const Complex &t)const {
		return {x-t.x,y-t.y};
	}
	Complex operator *(const Complex &t)const {
		return {x*t.x-y*t.y,x*t.y+y*t.x};
	}
}a[N],b[N];

int rev[N],bit,tot,res[N];

void fft(Complex a[],int inv){
	for(int i=0;i<tot;i++)
		if(i<rev[i])
			swap(a[i],a[rev[i]]);
	for(int mid=1;mid<tot;mid<<=1){
		auto w1=Complex({cos(PI/mid),inv*sin(PI/mid)});
		for(int i=0;i<tot;i+=mid*2){
			auto wk=Complex({1,0});
			for(int j=0;j<mid;j++,wk=wk*w1){
				auto x=a[i+j],y=wk*a[i+j+mid];
				a[i+j]=x+y,a[i+j+mid]=x-y;
			}
		}
	}
}
string s1,s2;
void solve(){
	cin>>s1>>s2;
	n=s1.size()-1;
	m=s2.size()-1;
	for(int i=0;i<=n;i++)a[i].x=s1[n-i]-'0';
	for(int i=0;i<=m;i++)b[i].x=s2[m-i]-'0';
	while((1<<bit)<n+m+1)bit++;
	tot=1<<bit;
	for(int i=0;i<tot;i++)
		rev[i]=(rev[i>>1]>>1)|((i&1)<<(bit-1));
	fft(a,1),fft(b,1);
	for(int i=0;i<tot;i++)a[i]=a[i]*b[i];
	fft(a,-1);
    int k=0;
    for(int i=0,t=0;i<tot||t;i++){
        t+=a[i].x/tot+0.5;
        res[k++]=t%10;
        t/=10;
    }
    while(k>1&&!res[k-1])k--;
    for(int i=k-1;i>=0;i--)cout<<res[i];
	
}

signed main()
{
	ios::sync_with_stdio(false);
    cin.tie(nullptr);
	int T=1;
//	cin>>T;
	while(T--)
		solve();
	return 0;
}
```





## 【模板】多项式乘法（FFT）
```cpp
#include<bits/stdc++.h>
#define x first
#define y second
#define int long long 
#define endl '\n'
using namespace std;
typedef pair<int,int>PII;

const int N=3000006;
const double PI=acos(-1);

int n,m;
int rev[N],bit,tot,res[N];

struct Complex{
	double x,y;
	Complex operator +(const Complex &t)const {
		return {x+t.x,y+t.y};
	}
	Complex operator -(const Complex &t)const {
		return {x-t.x,y-t.y};
	}
	Complex operator *(const Complex &t)const {
		return {x*t.x-y*t.y,x*t.y+y*t.x};
	}
}a[N],b[N];

void fft(Complex a[],int inv){
	for(int i=0;i<tot;i++)
		if(i<rev[i])
			swap(a[i],a[rev[i]]);
	for(int mid=1;mid<tot;mid<<=1){
		auto w1=Complex({cos(PI/mid),inv*sin(PI/mid)});
		for(int i=0;i<tot;i+=mid*2){
			auto wk=Complex({1,0});
			for(int j=0;j<mid;j++,wk=wk*w1){
				auto x=a[i+j],y=wk*a[i+j+mid];
				a[i+j]=x+y,a[i+j+mid]=x-y;
			}
		}
	}
}

void solve(){
	cin>>n>>m;
	for(int i=n;i>=0;i--)cin>>a[i].x;
	for(int i=m;i>=0;i--)cin>>b[i].x;
	while((1<<bit)<n+m+1)bit++;
	tot=1<<bit;
	for(int i=0;i<tot;i++)
		rev[i]=(rev[i>>1]>>1)|((i&1)<<(bit-1));
	fft(a,1),fft(b,1);
	for(int i=0;i<tot;i++)a[i]=a[i]*b[i];
	fft(a,-1);
	for(int i=n+m;i>=0;i--)
		cout<<(int)(a[i].x/tot+0.5)<<' ';
}

signed main()
{
	ios::sync_with_stdio(false);
    cin.tie(nullptr);
	int T=1;
//	cin>>T;
	while(T--)
		solve();
	return 0;
}

```
