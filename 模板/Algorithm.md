# 字符串

## 哈希

```cpp
//初始化
void init(ull *p,int len,int P){
	p[0]=1;
	for(int i=1;i<len;i++)p[i]=p[i-1]*P;
}

//下标从1开始
void InitHash(ull *h,char *s,int P){
	h[0]=1;
	int n=strlen(s+1);
	for(int i=1;i<=n;i++)h[i]=h[i-1]*P+s[i];
}

//哈希值
ull GetHash(int l,int r){
    return h[r]-h[l-1]*p[r-l+1];	
}

```

## KMP
```cpp
const int N = 100010;
int ne[N];
char p[N];
int n;
void init(){
	// 下标从1开始，得到母串ne数组
    for(int i=2,j=0;i<=n;i++)
    {
        while(j&&p[i]!=p[j+1])j=ne[j];
        if(p[i]==p[j+1])j++;
        ne[i]=j;
    }
}
```
## Manacher
```cpp
const int N=2e7+10;
int n;
char a[N],b[N];
int p[N];

void init()
{
    int k=0;
    b[k++]='$',b[k++]='#';
    for(int i=0;i<n;i++)b[k++]=a[i],b[k++]='#';
    b[k++]='^';
    n=k;
}

void manacher()
{
    int mr=0,mid;
    for(int i=1;i<n;i++)
    {
        if(i<mr)p[i]=min(p[mid*2-i],mr-i);
        else p[i]=1;
        while(b[i-p[i]]==b[i+p[i]])p[i]++;
        if(i+p[i]>mr)
        {
            mr=i+p[i];
            mid=i;
        }
    }
}
```
## 扩展KMP
```cpp
const int N=2e7+5;

int z[N],pl;
char p[N];

void getZ(){
	z[0]=pl;
	int now=0;
	while(now+1<pl&&p[now]==p[now+1])now++;
	z[1]=now;
	int p0=1;
	for(int i=2;i<pl;i++){
		if(i+z[i-p0]<p0+z[p0]) z[i]=z[i-p0];
		else{
			now=p0+z[p0]-i;
			now=max(now,0ll);
			while(now+i<pl&&p[now]==p[now+i])now++;
			z[i]=now;
			p0=i;
		}
	}
}
```
## AC自动机
```cpp
const int N= ,M= ;

int h[N*80],ne[N*80],e[N*80],cnt;
int n,tr[N*80][26],fail[N*80],tot,suc[N*80],sz[N*80];
char s[N][80],t[M];
queue<int> q;
pair<int,int> ans[N];

//遍历并统计数量
void dfs(int u){
	for(int i=h[u];~i;i=ne[i]){
		dfs(e[j]);
		sz[u]+=sz[j];
	}
}
void add(int u,int v){
	e[idx]=b,ne[idx]=h[u];h[u]=idx++;
}
void init(){
    memset(tr,0,sizeof(tr));
    memset(suc,0,sizeof(suc));
    memset(siz,0,sizeof(siz));
    memset(head,0,sizeof(head));
    tot=1;
    cnt=0;	
	for(i=0;i<26;++i)tr[0][i]=1;
}

// 插入
void insert(char *s){
    ans[i].first = 0;
    ans[i].second = i;
    for (j = 0, u = 1; s[i][j]; ++j){
        int c = s[i][j] - 'a';
        if (!tr[u][c]) tr[u][c] = ++tot;
        u = tr[u][c];
    }
    suc[u] = i;	
}

// 建立
void build(){
    q.push(1);
    while(!q.empty()){
        u=q.front();
        q.pop();
        for (i=0;i<26;++i){
            if(tr[u][i]){
                fail[tr[u][i]]=tr[fail[u]][i];
                q.push(tr[u][i]);
            }
            else tr[u][i]=tr[fail[u]][i];
        }
    }	
}

//查询
void query(char *t){
    for(i=0,u=1;t[i];++i){
        u=tr[u][t[i]-'a'];
        ++siz[u];
	}
    for(i=2;i<=tot;++i)add(fail[i],i);
    dfs(1);
	
}
```
## SA
```cpp
const int N=;
int n,m=;
char s[N];
int sa[N],rk[N],x[N],y[N],c[N],height[N];

void get_sa(){
    for(int i=1;i<=n;i++)c[x[i]=s[i]]++;
    for(int i=2;i<=m;i++)c[i]+=c[i-1];
    for(int i=n;i;i--)sa[c[x[i]]--]=i;
    for(int k=1;k<=n;k<<=1){
        int num=0;
        for(int i=n-k+1;i<=n;i++)y[++num]=i;
        for(int i=1;i<=n;i++)if(sa[i]>k)y[++num]=sa[i]-k;
        for(int i=1;i<=m;i++)c[i]=0;
        for(int i=1;i<=n;i++)c[x[i]]++;
        for(int i=2;i<=m;i++)c[i]+=c[i-1];
        for(int i=n;i;i--)sa[c[x[y[i]]]--]=y[i],y[i]=0;
        swap(x,y);
        x[sa[1]]=1,num=1;
        for(int i=2;i<=n;i++)
            x[sa[i]]=(y[sa[i]]==y[sa[i-1]]&&y[sa[i]+k]==y[sa[i-1]+k]?num:++num);
        if(num==n)break;
        m=num;
    }
}

void get_height(){
    for(int i=1;i<=n;i++)rk[sa[i]]=i;
    for(int i=1,k=0;i<=n;i++){
        if(rk[i]==1)continue;
        if(k)k--;
        int j=sa[rk[i]-1];
        while(i+k<=n&&j+k<=n&&s[i+k]==s[k+j])k++;
        height[rk[i]]=k;
    }
}
```
## SAM
```cpp
const int N=2000010;

int tot=1,last=1;

struct Node{
    int len,fa;
    int ch[26];
}node[N];

string str;
int f[N],ans;
int h[N],e[N],ne[N],idx;

void extend(int c){
    int p=last,np=last=++tot;
    f[tot]=1;
    node[np].len=node[p].len+1;
    for(;p&&!node[p].ch[c];p=node[p].fa)node[p].ch[c]=np;
    if(!p)node[np].fa=1;
    else{
        int q=node[p].ch[c];
        if(node[q].len==node[p].len+1)node[np].fa=q;
        else{
            int nq=++tot;
            node[nq]=node[q],node[nq].len=node[p].len+1;
            node[q].fa=node[np].fa=nq;
            for(;p&&node[p].ch[c]==q;p=node[p].fa)node[p].ch[c]=nq;
        }
    }
}

int n,id[N],x[N];

void init(){
	n=s.size();
	for(int i=1;i<=tot;i++)x[node[i].len]++;
	for(int i=1;i<=n;i++)x[i]+=x[i-1];
	for(int i=tot;i;i--)id[x[node[i].len]--]=i;
	for(int i=tot;i;i--)f[node[id[i]].fa]+=f[id[i]];
}

```
# 数据结构

