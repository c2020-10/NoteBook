# SA(后缀数组)

## 定义

后缀数组由两个数组构成，一个是$sa$，一个是$rk$
$sa_i$表示后缀排名第$i$小的后缀编号，$rk_i$表示后缀$i$的排名

## 实现

```cpp
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


## 应用

### 寻找最小循环移动位置
[洛谷 P4051 [JSOI2007]字符加密](https://www.luogu.com.cn/problem/P4051)
字符串复制一遍之后直接跑后缀排序

### 在字符串中寻找模式串
在主串$T$中寻找模式串$S$,二分即可
### 两个子串的最长公共前缀
对 $height$ 做 $RMQ$ 即可
