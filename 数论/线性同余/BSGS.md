# Random Number Generator

题意：给定$a, b, m, x0 ,x$问用$a_{n+1}= (a_n +b)\%m$得到的序列中有无出现$x$

柿子$X_{n+1}=(aX_n+b)\%m$

展开得$X_{n+1}=(a*(a*(a*(...*(aX_0+b)...)+b)+b)+b)\%m$

化简得：
	$$X_{n-1}=X_0a^n+a^{n-1}b+...+ab+b$$

再次化简可得
	$$X=X_0a^n+\frac{1-a^n}{1-a}b$$
	
不够明显再稍微划几下
$$X=X_0a^n+\frac{1}{1-a}b+\frac{a^n}{a-1}b$$

$$(X_0+\frac{b}{a-1})a^n=x+\frac{b}{a-1}$$

$$a^n=\frac{x+\frac{b}{a-1}}{X_0+\frac{b}{a-1}}$$

$$a^n\equiv\frac{x+\frac{b}{a-1}}{X_0+\frac{b}{a-1}}(\%m)$$

剩下的就是BSGS板子了

```cpp
#include<bits/stdc++.h>
#define x first
#define y second
#define int long long 
#define endl '\n'
#define all(x) (x).begin(),(x).end()
using namespace std;
typedef pair<int,int>PII;

int a,b,MOD,x,x0;

int fpow(int a,int b,int res=1){
	while(b){
		if(b&1)res=res*a%MOD;
		a=a*a%MOD;
		b>>=1;
	}
	return res;
}

const int INF=1e8;
int exgcd(int a,int b,int &x,int &y)
{
    if(!b)
    {
        x=1,y=0;
        return a;
    }
    int d=exgcd(b,a%b,y,x);
    y-=a/b*x;
    return d;
}
int bsgs(int a,int b,int p)
{
    if(1%p==b%p)return 0;
    int k=sqrt(p)+1;
    unordered_map<int,int>hash;
    for(int i=0,j=b%p;i<k;i++)
    {
        hash[j]=i;
        j=j*a%p;
    }
    int ak=1;
    for(int i=0;i<k;i++)ak=ak*a%p;
    for(int i=1,j=ak;i<=k;i++)
    {
        if(hash.count(j))return i*k-hash[j];
        j=j*ak%p;
    }
    return -INF;
}
int exbsgs(int a,int b,int p)
{
    b=(b%p+p)%p;
    if(1%p==b%p)return 0;
    int x,y;
    int d=exgcd(a,p,x,y);
    if(d>1)
    {
        if(b%d)return -INF;
        exgcd(a/d,p/d,x,y);
        return exbsgs(a,b/d*x%(p/d),p/d)+1;
    }
    return bsgs(a,b,p);
}

void solve(){
	cin>>a>>b>>MOD>>x0>>x;
	int g=b*fpow(a-1,MOD-2);
	int z=(x+g)%MOD*fpow((x0+g)%MOD,MOD-2)%MOD;
	int q;
	if(x==x0) q=1;
	else if(a==0) {
		if(b==x) q=1;
		else q=-1;
	}else if(a==1) {
		if(b) q=1;
		else q=-1;
	}
    else q=exbsgs(a,z,MOD);
    if(q<0)cout<<"NO"<<endl;
    else cout<<"YES"<<endl;	
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