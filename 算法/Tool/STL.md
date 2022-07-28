# STL 在算法中的应用

## 分类
STL 可分为容器（containers）、迭代器（iterators）、空间配置器（allocator）、配接器（adapters）、算法（algorithms）、仿函数（functors）六个部分。

## 命名空间

```cpp
using namespace std;
```

### example

不含`using namespace std;`

```cpp
#include<bits/stdc++.h>
int main(){
	std::vector<int>v(5,0);
	for(int i=0;i<5;i++)
		printf("%d ",v[i]);
}
```

含`using namespace std;`

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
	vector<int>v(5,0);
	for(int i=0;i<5;i++)
		printf("%d ",v[i]);
}
```

## 常用STL

### 迭代器(iterators)

迭代器是用于访问 STL 容器中元素的一种数据类型，一般迭代器的声明如下：

```
std::CONTAINER<T>::iterator p;
```

上述代码声明了一个迭代器 `p`，其中 `CONTAINER` 是容器类型，可以是 `vector`、`set` 等，`T` 是容器中元素的类型。

一般的，容器的 `begin()` 方法返回**首个元素**的迭代器，`end()` 方法返回**最后一个元素之后**的迭代器。这两个迭代器确定了一个包含容器内所有元素的**左闭右开区间** `[begin(), end())`。对于任何指向有效元素的迭代器都有其**不等于** `end()`，`end()` 并不指向任何一个元素，试图访问 `end()` 对应的元素是非法的。

```
for (std::CONTAINER<T>::iterator p = C.begin(); p != C.end(); p++) {
    std::cout << *p << std::endl;
}
```

### 容器(containers)

- 数组vector

```cpp
#include<bits/stdc++.h>
using namespace std;

struct Point{
	int x,y,z;
};

int main(){
	// vector<int>v(5,0);
	
	int n;
	cin>>n;

	//静态扩充
	vector<int>v(n,0);
	cout<<"数组:";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];
	cout<<endl;

	// 下标访问
	for(int i=0;i<n;i++)v[i]=i;
	cout<<"下标访问数组:";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];	
	cout<<endl;

	//数组清空
	v.clear();
	cout<<"清空数组：";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];
	cout<<endl;
	cout<<endl;
	
	//动态添加元素
	for(int i=0;i<n;i++)v.push_back(i);
	cout<<"动态添加:";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];
	cout<<endl;
	
	//数组大小
	cout<<"数组大小:"<<v.size()<<endl;
	cout<<endl;
	
	//数组最后一个元素
	cout<<"数组最后一个元素:"<<v.back()<<endl;
	cout<<endl;	
	
	//删除数组最后一个元素
	v.pop_back();
	cout<<"数组:";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];
	cout<<endl;
	
	//起始 or 末尾迭代器
	
	// cout<<*v.begin()<<endl;
	// cout<<*(v.end()-1)<<endl;
	
	// cout<<endl;
	
	//指定位置插入
	v.insert(v.begin()+1,6);
	cout<<"数组:";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];
	cout<<endl;	
	
	//指定位置删除
	v.erase(v.begin()+1);
	cout<<"数组:";
	for(int i=0;i<v.size();i++)cout<<v[i]<<" \n"[i==(v.size()-1)];
	cout<<endl;		
	
	
	//结构体
	vector<Point>d;
	d.push_back({1,2,3});
	cout<<"遍历结构体:";
	for(int i=0;i<1;i++)cout<<d[i].x<<' '<<d[i].y<<' '<<d[i].z<<endl;
	
}
```

- 集合set

STL 在头文件 `<set>` 中提供了一个**有序集合** `set`，其中的元素全部是**唯一**的，并且插入进的元素自动按照升序排列，但 `set` 不支持通过下标定位某个元素，只能通过**迭代器**遍历。

以下代码声明了一个 `int` 类型的集合。

```
std::set<int> s;
```

使用 `insert()` 在集合中加入一个元素，其时间复杂度为。

使用 `erase()` 删除集合中**某个元素**或**某个位置的元素**，其时间复杂度均为。

`set` 自身提供 `lower_bound()` 用于定位元素，其作用与前文中的同名函数类似，也可以使用 `find()` 来精确查找元素。

遍历 `set` 只能使用**迭代器**。`set` 的迭代器为 `set<T>::iterator`，其中 `T` 为元素类型。

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
	set<int>s;
	//插入
	for(int i=1;i<=5;i++)s.insert(i);
	//遍历
	for(auto x:s)cout<<x<<' ';
	cout<<endl;

	//重复插入无效
	for(int i=1;i<=5;i++)s.insert(i);	
	//遍历
	for(auto x:s)cout<<x<<' ';
	
	cout<<endl;
	//删除
	for(int i=1;i<=3;i++)s.erase(i);	

	cout<<endl;
	//遍历
	for(auto x:s)cout<<x<<' ';
}
```

- 字典map
map是一类关联式容器。它的特点是增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。

对于迭代器来说，可以修改实值，而不能修改key。



```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
	//统计数字
	map<int,int>Cnum;
	for(int i=1;i<=100;i++){
		int t=i;
		while(t)Cnum[t%10]++,t/=10;
	}
	for(int i=0;i<10;i++)cout<<i<<':'<<Cnum[i]<<endl;
	
	cout<<endl;
	//统计字符
	string s("asdasdasfasfasfas");
	map<char,int>Cchar;
	for(auto c:s)Cchar[c]++;
	// <key,value>
	for(auto t:Cchar){
		cout<<t.first<<':'<<t.second<<endl;
	}
}
```

- 队列 queue

STL 在头文件 `<queue>` 中提供了先入先出（FIFO）队列 `queue`。

使用 `push()` 向队列中加入元素。

使用 `front()` 获取队首元素（并不删除）。

使用 `pop()` 删除队首元素。

使用 `empty()` 判断队列是否为空。

```
#include<bits/stdc++.h>
using namespace std;

int main(){
	queue<int>q;
	
	
	for(int i=1;i<=9;i++)q.push(i);
	// 1
	// 1 2
	// 1 2 3
	// 1 2 3 4
	// 1 2 3 4 5
	// 1 2 3 4 5 6 
	// 1 2 3 4 5 6 7
	// 1 2 3 4 5 6 7 8 
	// 1 2 3 4 5 6 7 8 9 
	while(!q.empty()){
		cout<<q.front()<<' ';
		q.pop();
	}
	
}
```


- 栈stack

STL 在头文件 `<stack>` 提供了后入先出（LIFO）栈 `stack`。

使用 `push()` 向栈中加入元素。

使用 `top()` 获取栈顶元素（并不删除）。

使用 `pop()` 删除栈顶元素。

使用 `empty()` 判断栈是否为空。

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
	stack<int>q;
	
	
	for(int i=1;i<=9;i++)q.push(i);
	// 1
	// 1 2
	// 1 2 3
	// 1 2 3 4
	// 1 2 3 4 5
	// 1 2 3 4 5 6 
	// 1 2 3 4 5 6 7
	// 1 2 3 4 5 6 7 8 
	// 1 2 3 4 5 6 7 8 9 
	while(!q.empty()){
		cout<<q.top()<<' ';
		q.pop();
	}
	
}
```

- 优先队列 priority_queue

STL 在头文件 `<queue>` 中提供优先队列 `priority_queue`，在任意时间都能取出队列中的**最大值**。

使用 `push()` 向优先队列中加入元素，其时间复杂度为。

使用 `top()` 获取优先队列中**最大**的元素（并不删除），其时间复杂度为。

使用 `pop()` 删除优先队列中**最大**元素，其时间复杂度为。

使用 `empty()` 判断优先队列是否为空。

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
	priority_queue<int>q;
	for(int i=1;i<=9;i++)q.push(i);
	for(int i=1;i<=9;i++)q.push(i);
	while(!q.empty()){
		cout<<q.top()<<endl;
		q.pop();
	}
}
```

- 双端队列deque

使用 `push_front()` 向队头加入元素。

使用 `push_back()` 向队尾加入元素。

使用 `front()` 获取队首元素（并不删除）。

使用 `back()` 获取队尾元素（并不删除）。

使用 `pop_front()` 删除队首元素。

使用 `pop_back()` 删除队尾元素。

使用 `empty()` 判断队列是否为空。

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
	deque<int>q;
	for(int i=1;i<=4;i++)q.push_back(i);
	//1 2 3 4 5 6 7 8 9
	for(int i=1;i<=4;i++)q.push_front(i);
	//9 8 7 6 5 4 3 2 1 1 2 3 4 5 6 7 8 9
	while(!q.empty()){
		cout<<q.back();
		q.pop_back();
	}
	//43211234
	
	// while(!q.empty()){
		// cout<<q.front()<<endl;
		// q.pop_front();
	// }
	//43211234
}

```

### 算法(Algorithm)


- 排序sort

```cpp
#include<bits/stdc++.h>
using namespace std;

int a[10]={1,5,6,4,7,9,2,3,8,0};

int main(){
	for(int i=0;i<10;i+=1)cout<<a[i]<<" \n"[i==9];
	//默认从小到大
	sort(a,a+10);
	cout<<endl;
	for(int i=0;i<10;i+=1)cout<<a[i]<<" \n"[i==9];
	//函数决定顺序
	sort(a,a+10,greater<int>());
	cout<<endl;
	for(int i=0;i<10;i+=1)cout<<a[i]<<" \n"[i==9];
}
```

- 去重unique
```cpp
#include<bits/stdc++.h>
using namespace std;

int s[10]={1,5,6,4,4,0,2,6,8,0};

int main(){
	vector<int>a;
	for(int i=0;i<10;i++)a.push_back(s[i]);
	for(auto x:a)cout<<x<<' ';
	cout<<endl<<endl;
	sort(a.begin(),a.end());
	unique(a.begin(),a.end());
	//去重
	for(auto x:a)cout<<x<<' ';
	cout<<endl<<endl;
	
	//删除重复的元素
	sort(a.begin(),a.end());	
	a.erase(unique(a.begin(),a.end()),a.end());
	
	for(auto x:a)cout<<x<<' ';
	cout<<endl;

}
```

- 较大、较小值
```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
	//两数找最大
	int Max=max(2,1);
	cout<<Max<<endl;
	
	//多个数找最大
	Max=max({1,2,3,4,5,9});
	cout<<Max<<endl;	
	
	cout<<endl;
	
	//找最小与之类似
	int Min=min(2,1);
	cout<<Min<<endl;
	
	Min=min({1,2,3,4,5,9});
	cout<<Min<<endl;		
}
```

- 查找 

常用函数`lower_bound`、`upper_bound`

```cpp
#include<bits/stdc++.h>
using namespace std;

int s[10]={0,1,1,5,6,7,8,9,10,11};

int main(){
	
	//lower_bound 找到第一个大于等于x的元素,返回迭代器
	
	int x=5;
	
	cout<<lower_bound(s,s+10,x)-s<<' '<<*lower_bound(s,s+10,x)<<endl;

	//upper_bound 找到第一个大于x的元素,返回迭代器
	
	
	cout<<upper_bound(s,s+10,x)-s<<' '<<*upper_bound(s,s+10,x)<<endl;
	
}
```

- 交换
```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
	
	int a=3,b=5;
	swap(a,b);
	cout<<a<<' '<<b<<endl;
}
```

## 练习

- 查找 
	https://www.luogu.com.cn/problem/P2249
- 栈
	https://www.luogu.com.cn/problem/B3614
- 队列
	https://www.luogu.com.cn/problem/B3616
- 堆
	https://www.luogu.com.cn/problem/P3378