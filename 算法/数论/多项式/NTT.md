# 快速数论变换
**数论变换**(Number-theoretic transform, NTT）是 快速傅里叶变换（FFT）在数论基础上的实现。

NTT 解决的是多项式乘法带模数的情况，可以说有些受模数的限制，数也比较大，

## 定义

### 数论变换
**数论变换** 是一种计算卷积（convolution）的快速算法。最常用算法就包括了前文提到的快速傅里叶变换。然而快速傅立叶变换具有一些实现上的缺点，举例来说，资料向量必须乘上复数系数的矩阵加以处理，而且每个复数系数的实部和虚部是一个正弦及余弦函数，因此大部分的系数都是浮点数，也就是说，必须做复数而且是浮点数的运算，因此计算量会比较大，而且浮点数运算产生的误差会比较大。

在数学中，NTT 是关于任意上的离散傅立叶变换（DFT）。在有限域的情况下，通常称为数论变换 (NTT)。

### 离散傅里叶变换

**离散傅里叶变换**(Discrete Fourier transform，DFT) 是傅里叶变换在时域和频域上都呈离散的形式，将信号的时域采样变换为其 DTFT 的频域采样。

```cpp
#include<bits/stdc++.h>
#define x first
#define y second
#define int long long 
#define endl '\n'
using namespace std;
typedef pair<int,int>PII;

const int N=3000002,P=998244353;

int A[N],B[N],C[N],r[N];
string a,b;

int qmi(int a,int b,int res=1){
	while(b){
		if(b&1)res=res*a%P;
		a=a*a%P;
		b>>=1;
	}
	return res;
}

void ntt(int *x,int lim,int opt){
	for(int i=0;i<lim;i++)
		if(r[i]<i)swap(x[r[i]],x[i]);
	for(int m=2;m<=lim;m<<=1){
		int k=m>>1;
		int gn=qmi(3,(P-1)/m);
		for(int i=0;i<lim;i+=m){
			int g=1;
			for(int j=0;j<k;g=g*gn%P,j++){
				int tmp=x[i+j+k]*g%P;
				x[i+j+k]=(x[i+j]-tmp+P)%P;
				x[i+j]=(x[i+j]+tmp)%P;
			}
		}
	}
	if(opt==-1){
		reverse(x+1,x+lim);
		int inv=qmi(lim,P-2);
		for(int i=0;i<lim;i++)x[i]=x[i]*inv%P;
	}
}

void solve(){
	int lim=1,n;
	cin>>a;
	n=a.size();
	for(int i=0;i<n;i++)A[i]=a[n-i-1]-'0';
	while(lim<(n<<1))lim<<=1;
	cin>>b;
	n=b.size();
	for(int i=0;i<n;i++)B[i]=b[n-i-1]-'0';
	while(lim<(n<<1))lim<<=1;
	for(int i=0;i<lim;i++)r[i]=(i&1)*(lim>>1)+(r[i>>1]>>1);
	ntt(A,lim,1);
	ntt(B,lim,1);
	for(int i=0;i<lim;i++)C[i]=A[i]*B[i]%P;
	ntt(C,lim,-1);
	int len=0;
	for(int i=0;i<lim;i++){
		if(C[i]>=10)len=i+1,C[i+1]+=C[i]/10,C[i]%=10;
		if(C[i])len=max(len,i);
	}
	while(C[len]>=10)C[len+1]+=C[len]/10,C[len]%=10,len++;
	for(int i=len;~i;i--)cout<<C[i];
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

## Hash Function
```cpp
// Problem: Hash Function
// Contest: NowCoder
// URL: https://ac.nowcoder.com/acm/contest/11166/H
// Memory Limit: 524288 MB
// Time Limit: 4000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
#define x first
#define y second
#define endl '\n'
#define all(x) (x).begin(),(x).end()
using namespace std;
typedef pair<int,int>PII;
typedef unsigned long long ull;
 
const int P = 998244353;
 
int norm(int x) { return x >= P ? x - P : x; }
int reduce(int x) { return x < 0 ? x + P : x; }
int neg(int x) { return x ? P - x : 0; }
void add(int& x, int y) { if ((x += y - P) < 0) x += P; }
void sub(int& x, int y) { if ((x -= y) < 0) x += P; }
void fam(int& x, int y, int z) { x = (x + y * (ull)z) % P; }
int mpow(int x, unsigned k) {
    if (k == 0) return 1;
    int ret = mpow(x * (ull)x % P, k >> 1);
    if (k & 1) ret = ret * (ull)x % P;
    return ret;
}
 
 
const int L = 20;


namespace NTT {
 
    const int OMEGA_2_21 = 31;
 
    int l, n;
    int w2[(1 << L) + 1];
 
    void init() {
        n = 1 << l;
        *w2 = 1;
        w2[1 << l] = mpow(OMEGA_2_21, 1 << (21 - l));
        for (int i = l; i; --i) w2[1 << (i - 1)] = w2[1 << i] * (ull)w2[1 << i] % P;
        for (int i = 1; i < n; ++i) w2[i] = w2[i & (i - 1)] * (ull)w2[i & -i] % P;
    }
 
    void DIF(int *a) {
        int i, *j, *k, len = n >> 1, r, *o;
        for (i = 0; i < l; ++i, len >>= 1)
            for (j = a, o = w2; j != a + n; j += len << 1, ++o)
                for (k = j; k != j + len; ++k)
                    r = *o * (ull)k[len] % P, k[len] = reduce(*k - r), add(*k, r);
    }
 
    void DIT(int *a) {
        int i, *j, *k, len = 1, r, *o;
        for (i = 0; i < l; ++i, len <<= 1)
            for (j = a, o = w2; j != a + n; j += len << 1, ++o)
                for (k = j; k != j + len; ++k)
                    r = reduce(*k + k[len] - P), k[len] = ull(*k - k[len] + P) * *o % P, *k = r;
    }
 
    void FFT(int* a, int d = 1) {
        if (d == 1) DIF(a);
            else {
                DIT(a);
                std::reverse(a + 1, a + n);
                ull nv = P - (P - 1) / n;
                for (int i = 0; i < n; ++i) a[i] = a[i] * nv % P;
            }
    }
 
}

using NTT::l;

int n,m;
int a[1<<L],b[1<<L];

void solve(){
	cin>>n;
	while(n--){
		int ai;
		cin>>ai;
		a[ai]=1;
		m=max(m,ai);
	}
	++m;
	while((1<<l)<=m*2)++l;
	NTT::init();
	memcpy(b,a,sizeof a);
	reverse(b+1,b+(1<<l));
	NTT::FFT(a),NTT::FFT(b);
	for(int i=0;i!=1<<l;i++)a[i]=a[i]*(ull)b[i]%P;
	NTT::FFT(a,-1);
	for(int i=1;i<=m;i++){
		bool fl=false;
		for(int j=i;j<=m;j+=i)fl|=a[j];
		if(!fl){
			cout<<i<<endl;
			break;
		}
	}
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