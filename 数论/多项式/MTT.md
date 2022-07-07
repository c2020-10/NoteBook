# NTT
```cpp
#include <algorithm>
#include <cstdio>
#include <cstring>
int mod;
namespace Math {
	inline int pw(int base, int p, const int mod) {
		static int res;
		for (res = 1; p; p >>= 1, base = static_cast<long long> (base) * base % mod) if (p & 1) res = static_cast<long long> (res) * base % mod;
		return res;
	}
	inline int inv(int x, const int mod) { return pw(x, mod - 2, mod); }
}

const int mod1 = 998244353, mod2 = 1004535809, mod3 = 469762049, G = 3;
const long long mod_1_2 = static_cast<long long> (mod1) * mod2;
const int inv_1 = Math::inv(mod1, mod2), inv_2 = Math::inv(mod_1_2 % mod3, mod3);
struct Int {
	int A, B, C;
	explicit inline Int() { }
	explicit inline Int(int __num) : A(__num), B(__num), C(__num) { }
	explicit inline Int(int __A, int __B, int __C) : A(__A), B(__B), C(__C) { }
	static inline Int reduce(const Int &x) {
		return Int(x.A + (x.A >> 31 & mod1), x.B + (x.B >> 31 & mod2), x.C + (x.C >> 31 & mod3));
	}
	inline friend Int operator + (const Int &lhs, const Int &rhs) {
		return reduce(Int(lhs.A + rhs.A - mod1, lhs.B + rhs.B - mod2, lhs.C + rhs.C - mod3));
	}
	inline friend Int operator - (const Int &lhs, const Int &rhs) {
		return reduce(Int(lhs.A - rhs.A, lhs.B - rhs.B, lhs.C - rhs.C));
	}
	inline friend Int operator * (const Int &lhs, const Int &rhs) {
		return Int(static_cast<long long> (lhs.A) * rhs.A % mod1, static_cast<long long> (lhs.B) * rhs.B % mod2, static_cast<long long> (lhs.C) * rhs.C % mod3);
	}
	inline int get() {
		long long x = static_cast<long long> (B - A + mod2) % mod2 * inv_1 % mod2 * mod1 + A;
		return (static_cast<long long> (C - x % mod3 + mod3) % mod3 * inv_2 % mod3 * (mod_1_2 % mod) % mod + x) % mod;
	}
} ;

#define maxn 131072

namespace Poly {
#define N (maxn << 1)
	int lim, s, rev[N];
	Int Wn[N | 1];
	inline void init(int n) {
		s = -1, lim = 1; while (lim < n) lim <<= 1, ++s;
		for (register int i = 1; i < lim; ++i) rev[i] = rev[i >> 1] >> 1 | (i & 1) << s;
		const Int t(Math::pw(G, (mod1 - 1) / lim, mod1), Math::pw(G, (mod2 - 1) / lim, mod2), Math::pw(G, (mod3 - 1) / lim, mod3));
		*Wn = Int(1); for (register Int *i = Wn; i != Wn + lim; ++i) *(i + 1) = *i * t;
	}
	inline void NTT(Int *A, const int op = 1) {
		for (register int i = 1; i < lim; ++i) if (i < rev[i]) std::swap(A[i], A[rev[i]]);
		for (register int mid = 1; mid < lim; mid <<= 1) {
			const int t = lim / mid >> 1;
			for (register int i = 0; i < lim; i += mid << 1) {
				for (register int j = 0; j < mid; ++j) {
					const Int W = op ? Wn[t * j] : Wn[lim - t * j];
					const Int X = A[i + j], Y = A[i + j + mid] * W;
					A[i + j] = X + Y, A[i + j + mid] = X - Y;
				}
			}
		}
		if (!op) {
			const Int ilim(Math::inv(lim, mod1), Math::inv(lim, mod2), Math::inv(lim, mod3));
			for (register Int *i = A; i != A + lim; ++i) *i = (*i) * ilim;
		}
	}
#undef N
}

int n, m;
Int A[maxn << 1], B[maxn << 1];
int main() {
	scanf("%d%d%d", &n, &m, &mod); ++n, ++m;
	for (int i = 0, x; i < n; ++i) scanf("%d", &x), A[i] = Int(x % mod);
	for (int i = 0, x; i < m; ++i) scanf("%d", &x), B[i] = Int(x % mod);
	Poly::init(n + m);
	Poly::NTT(A), Poly::NTT(B);
	for (int i = 0; i < Poly::lim; ++i) A[i] = A[i] * B[i];
	Poly::NTT(A, 0);
	for (int i = 0; i < n + m - 1; ++i) {
		printf("%d", A[i].get());
		putchar(i == n + m - 2 ? '\n' : ' ');
	}
	return 0;
}
```

# FFT
```cpp
#include<cmath>
#include<cstdio>
#include<iostream>
#include<algorithm>

const int N=4e5+2;
const int BLOCK=32768;
const long double PI=acos(-1);

typedef long long ll;

int filp[N],Ans[N],limit=1,cnt=0,n,m,p;
struct complex{complex(long double a=0,long double b=0){x=a,y=b;}long double x,y;};
complex operator + (complex a,complex b){return complex(a.x+b.x,a.y+b.y);}
complex operator - (complex a,complex b){return complex(a.x-b.x,a.y-b.y);}
complex operator * (complex a,complex b){return complex(a.x*b.x-a.y*b.y,a.x*b.y+a.y*b.x);}
complex A1[N],B1[N],A2[N],B2[N],X[N];

inline void FFT(complex *f,short inv){
    for(int i=0;i<limit;++i)if(i<filp[i])std::swap(f[i],f[filp[i]]);
    for(int p=2;p<=limit;p<<=1){
        int len=p>>1;
        complex tmp=complex(std::cos(PI/len),inv*std::sin(PI/len));
        for(int k=0;k<limit;k+=p){
            complex buf=complex(1,0);
            for(int l=k;l<k+len;++l,buf=buf*tmp){
                complex t=f[l+len]*buf;
                f[l+len]=f[l]-t,f[l]=f[l]+t;
            }			
        }
    }return;
}

inline void solve(complex *a,complex *b,int res){
    for(int i=0;i<limit;++i)X[i]=a[i]*b[i];
    FFT(X,-1);
    for(int i=0;i<=n+m;++i)(Ans[i]+=(ll)(X[i].x/limit+0.5)%p*res%p)%=p;
}
inline void MTT(complex *a,complex *b,complex *c,complex *d){
    FFT(a,1),FFT(c,1),FFT(b,1),FFT(d,1);
    solve(a,c,BLOCK*BLOCK%p);solve(a,d,BLOCK%p);
    solve(c,b,BLOCK%p);solve(b,d,1);
}

int main(){
    scanf("%d%d%d",&n,&m,&p);
    for(int i=0,x;i<=n;++i)
        scanf("%d",&x),A1[i]=x/BLOCK,B1[i]=x%BLOCK;
    for(int i=0,x;i<=m;++i)
        scanf("%d",&x),A2[i]=x/BLOCK,B2[i]=x%BLOCK;
    for(limit=1;limit<=n+m;limit<<=1)++cnt;--cnt;
    for(int i=0;i<limit;++i)filp[i]=(filp[i>>1]>>1)|((i&1)<<cnt);
    MTT(A1,B1,A2,B2);
    for(int i=0;i<=n+m;++i)
        printf("%d ",Ans[i]);
    printf("\n");
    return 0;
}
```