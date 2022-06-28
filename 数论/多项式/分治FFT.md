```c++
#include<bits/stdc++.h>
#define x first
#define y second
#define int long long 
#define endl '\n'
using namespace std;
typedef pair<int,int>PII;

const int N=1e6+7,MOD=998244353;

int n,g[N],f[N],a[N<<2],b[N<<2],rev[N<<2],tot,bit;

int qmi(int a,int b,int mod,int res=1){
	while(b){
		if(b&1)res=res*a%mod;
		a=a*a%mod;
		b>>=1;
	}
	return res;
}

void NTT(int *a,int type){
    for(int i=0;i<tot;i++)if(i<rev[i])swap(a[i],a[rev[i]]);
    for(int mid=1;mid<tot;mid<<=1){
        int wn=qmi(3,type==1?(MOD-1)/(mid<<1):(MOD-1-(MOD-1)/(mid<<1)),MOD);
        for(int i=0;i<tot;i+=mid*2){
            int w=1;
            for(int j=0;j<mid;j++,w=1ll*w*wn%MOD){
                int x=a[i+j],y=1ll*w*a[i+j+mid]%MOD;
                a[i+j]=(x+y)%MOD,a[i+j+mid]=(x+MOD-y)%MOD;
            }
        }
    }
    if(type==-1){
        int inv=qmi(tot,MOD-2,MOD);
        for(int i=0;i<tot;i++)a[i]=1ll*a[i]*inv%MOD;
    }
}

void cal(int *a,int *b){
	NTT(a,1);
	NTT(b,1);
	for(int i=0;i<tot;i++)a[i]=(a[i]*b[i])%MOD;
	NTT(a,-1);
}

void cdqfft(int l,int r){
	if(l==r)return ;
	int mid=(r+l)>>1,len=(r-l-1);
	cdqfft(l,mid);
	bit=0,tot=1;
	while(tot<=len)tot<<=1,bit++;
	for(int i=0;i<tot;i++)rev[i]=(rev[i>>1]>>1)|((i&1)<<(bit-1)),a[i]=b[i]=0;
	for(int i=l;i<=mid;i++)a[i-l]=f[i];
	for(int i=1;i<=r-l;i++)b[i-1]=g[i];
	cal(a,b);
	for(int i=mid+1;i<=r;i++)f[i]=(f[i]+a[i-l-1])%MOD;
	cdqfft(mid+1,r);
}

void solve(){
	cin>>n;
	for(int i=1;i<n;i++)cin>>g[i];
	f[0]=1;
	cdqfft(0,n-1);
	for(int i=0;i<n;i++)cout<<f[i]<<' ';
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