> 出处：
>
> https://blog.csdn.net/qq_41461536/category_11986193.html
>
> C++中文在线手册：
>
> https://zh.cppreference.com/





# 宏定义

掌握"宏"概念的关键是“**换**”。一切以换为前提、做任何事情之前先要换！

## 不含参数

```c++
#define PI 3.1415926
```

把程序中出现的`PI`全部换成`3.1415926`

```
#define endl '\n'
```

把程序中出现的`endl`全部换成字符`'\n'`



## 含参数

### 例子1

```cpp
#include<iostream>
 
#define endl '\n'
#define SR(x, y) x+y
using namespace std;
 
void sum1() {
    int a = 5, b = 4;
    a += 2 * SR1(a, b);
    cout << "sum1=" << a << endl;//结果是19
}
```

第一步，**先换**：语句替换后成为`a+=2*a+b`。

第二步，替换变量，语句替换后成为`5+=2*5+4`。

第三步，计算式子，先是右侧公式计算结果为`14`，此时为`5+=14`；最后赋值到变量上，结果是**19**！

### 例子2

```cpp
#include<iostream>
#define endl '\n'
#define SR(x， y) ((x)+(y))
using namespace std;
 
void sum2() {
    int a = 5, b = 4;
    a += 2 * SR2(a, b);
    cout << "sum2=" << a << endl;//结果是23
}
```

第一步，**先换**：语句替换后成为`a+=2*((a)+(b))`。

第二步，替换变量，语句替换后成为`5+=2*(5+4)`。

第三步，计算式子，先是右侧公式计算结果为`18`，此时为`5+=18`；最后赋值到变量上，结果是**23**！

### 例子3

```c++
#define fun1(a) a
 
void f1() {
    cout << "f1=" << fun1(12 + 3)<< endl;//结果是数字15
}
 
#define fun2(a) "a"
 
void f2() {
    cout << "f2=" << fun2(12 + 3)<< endl;//结果是字符串a
}
```

请看清楚，定义的后面究竟是参数本身还是带着双引号的字符串！



## 含参循环

```c++
#include <iostream>
 
using namespace std;
 
// #define doit(m, n) for(int i=0;i<(n);++i){m+=i;}
#define doit(m,n) for(int i=0;i<(n);++i)\
{\
m+=i;\
}
 
int main() {
    int a = 5, b = 4;
    doit(a, b);
    cout << a;// 结果为11
    return 0;
}
```

宏定义也是可以分行的！！！

宏定义也是可以运行循环的！！！

宏定义也是能让人栽跟头的！！！



## 宏定义语法特点

### 含`#`和`##`

- 使用`#`把宏参数变为一个**字符串**
- 用`##`把两个宏参数**贴合在一起**

```c++
#include<iostream>
 
using namespace std;
 
#define endl '\n'
#define STR(s)     #s
#define CONS(a, b)  int(a##e##b)
 
int main() {
    cout << STR(vck)<< endl;       // 输出字符串"vck"
    cout << CONS(2, 3);  // 2e3 输出:2000
    return 0;
}
#undef CONS
#undef STR
```

有这种复杂需求的话还是写成函数更合适一些

### 特点

1. 宏定义末尾**不加分号**；
2. 宏定义写在函数的花括号外边，**作用域为其后的程序**，可以用`#undef`命令**终止作用域**
3. 宏定义**可以嵌套**
4. 字符串`""`中永远不包含宏
5. 宏定义**不分配内存**，想要分配内存去使用变量吧。
6. 预处理**不做语法检查**。因为预处理是在编译之前的处理，而编译工作的任务之一就是语法检查
7. 宏定义**不做计算**，只作替换
8. 宏的哑实结合**不存在类型**，也没有类型转换。



### 优点

使用宏可**提高程序的通用性和易读性，减少不一致性，减少输入错误和便于修改。**

例如：数组大小常用宏定义。



### 与函数的区别

1. - 函数调用是在编译后程序运行时进行，并且分配内存。
	- 宏替换在编译前进行，不分配内存
2. - 函数一般只有一个返回值，特殊情况下用`pair`也是一个对象，对象内两个值
	- 宏则可以设法得到多个值
3. 宏展开会使源程序变长，函数调用不会
4. - 函数调用**占运行时间**（分配内存、保留现场、值传递、返回值）
	- 宏展开不占运行时间，只**占编译时间**



# 命名空间

起因还是我做错了题，平时只是使用std来完成编码，没有细究过，老师也是一语带过。

等到了真正纠结变量作用域的时候，才发现命名空间里面大有学问。



## 命名空间

命名空间这个概念，作为附加信息来区分不同库中相同名称的函数、类、变量等。使用了命名空间即定义了上下文。**本质上，命名空间就是定义了一个范围。**



`using namespace` 指令，这样在使用命名空间时就可以不用在前面加上命名空间的名称。

可以使用 `using namespace` 指令，这样在使用命名空间时就可以不用在前面加上命名空间的名称。这个指令会告诉编译器，后续的代码将使用指定的命名空间中的名称。

```c++
#include <iostream>
 
using namespace std;
 
// 第一个命名空间
namespace first_space {
    void func() {
        cout << "Inside first_space" << endl;
    }
}
// 第二个命名空间
namespace second_space {
    void func() {
        cout << "Inside second_space" << endl;
    }
}
 
int main() {
 
    // 调用第一个命名空间中的函数
    first_space::func();
 
    // 调用第二个命名空间中的函数
    second_space::func();
 
    return 0;
}
```



## 同名变量

同名变量让人头疼，因为不同的范围内声明的不同类型同名变量会覆盖，而离开指定范围后又会变回去。

命名空间是一种区分的方法：

```c++
#include <iostream>
 
#define endl "\n"
using namespace std;
 
double a;//1号
 
int main() {
    int a = 10;//2号
    {
        int a = 20;//3号
        double b;
        ::a = 20.5;//1号
        b = ::a + a;//1号 +3号
        cout << "a=" << a << " " << "b=" << b << endl;//输出3号
        cout << "::a=" << ::a << endl;//输出1号
    }
    cout << "a=" << a << endl;//输出2号
    cout << "::a=" << ::a << endl;//输出1号
    return 0;
}
```

**全局变量 `a` 表达为 `::a`，用于当有同名的局部变量时来区别两者。**



## 不连续的命名空间

命名空间可以定义在几个不同的部分中，因此命名空间是由几个单独定义的部分组成的。一个命名空间的各个组成部分可以分散在多个文件中。

所以，如果命名空间中的某个组成部分需要请求定义在另一个文件中的名称，则仍然需要声明该名称。下面的命名空间定义可以是定义一个新的命名空间，也可以是为已有的命名空间增加新的元素：

```c++
namespace namespace_name {
   // 代码声明
}
```



## 嵌套的命名空间

命名空间可以嵌套，您可以在一个命名空间中定义另一个命名空间，如下所示：

```c++
namespace namespace_name1 {
   // 代码声明
   namespace namespace_name2 {
      // 代码声明
   }
}
```

可以通过使用 **::** 运算符来访问嵌套的命名空间中的成员：

```c++
// 访问 namespace_name2 中的成员
using namespace namespace_name1::namespace_name2;
 
// 访问 namespace_name1 中的成员
using namespace namespace_name1;
```

在上面的语句中，如果使用的是 `namespace_name1`，那么在该范围内 `namespace_name2` 中的元素也是可用的，如下所示：

```c++
#include <iostream>
using namespace std;
 
// 第一个命名空间
namespace first_space{
   void func(){
      cout << "Inside first_space" << endl;
   }
   // 第二个命名空间
   namespace second_space{
      void func(){
         cout << "Inside second_space" << endl;
      }
   }
}
using namespace first_space::second_space;
int main ()
{
 
   // 调用第二个命名空间中的函数
   func();
   
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Inside second_space
```



## 可能出现的情况

补充关于 using 的错误事例：

```c++
#include <iostream>
using namespace std;
namespace A
{
    int a = 100;
    int fun()
    {
        cout<<"a = "<<a<<endl;
    }

    namespace B            //嵌套一个命名空间B
    {
        int a =20;
        int fun()
        {
             cout<<"a = "<<a<<endl;
        }

    }
}

using namespace A::B;
int main(int argc, char *argv[])
{
    cout<<a<<endl;
    fun();

    return 0;
}
```

在 `main()` 上面加 `using namespace A;` 或者 `using namespace A::B;` 。这样就可以使用其中的 `a` 和 `fun()`。但是不能同时使用，因为这样也会导致编译出错，编译器器不知道要去使用哪个 `a` 和 `fun()`。



补充一个命名空间冲突的情况：

```c++
#include <iostream>

using namespace std;
namespace A {
    int a = 100;
    namespace B            //嵌套一个命名空间B
    {
        int a = 20;
    }
}

int a = 200;//定义一个全局变量

int main(int argc, char *argv[]) {
    cout << "A::a =" << A::a << endl;        //A::a =100
    cout << "A::B::a =" << A::B::a << endl;  //A::B::a =20
    cout << "a =" << a << endl;              //a =200
    cout << "::a =" << ::a << endl;          //::a =200

    using namespace A;
    cout << "a =" << a << endl;     // Reference to 'a' is ambiguous // 命名空间冲突，编译期错误
    cout << "::a =" << ::a << endl; //::a =200

    int a = 30;
    cout << "a =" << a << endl;     //a =30
    cout << "::a =" << ::a << endl; //::a =200

    //即：全局变量 a 表达为 ::a，用于当有同名的局部变量时来区别两者。

    using namespace A;
    cout << "a =" << a << endl;     // a =30  // 当有本地同名变量后，优先使用本地，冲突解除
    cout << "::a =" << ::a << endl; //::a =200

    return 0;
}
```



# queue

## 定义

```c++
#include <queue>
using namespace std;
 
queue<int> q;
```

## 常规操作

| 函数名  | 作用                 |
| :------ | :------------------- |
| back()  | 返回最后一个元素     |
| empty() | 如果队列空则返回真   |
| front() | 返回第一个元素       |
| pop()   | 删除第一个元素       |
| push()  | 在末尾加入一个元素   |
| size()  | 返回队列中元素的个数 |



```c++
#include <iostream>
#include <queue>

using namespace std;

int main() {
    queue<int> q;//空对象
    q.push(2);//尾部插入
    q.push(3);
    q.push(1);
    q.push(4);
    cout << q.size() << endl;//元素个数 
    if (!q.empty()) { //若true,则队列中无元素
        int temp = q.front(); //获取队头元素
        cout << temp << endl;
        temp = q.back(); //获取队尾元素
        cout << temp << endl;
        q.pop();//弹出队头元素
    }
    return 0;
}
```



# vector

能够在运行时高效地存放各种类型的**动态数组**`vector`！

## 定义

```c++
#include <Vector> 
using namespace std;
 
void init() {
 
    //空对象
    vector<int> v1;
 
    //元素个数为5，每个int元素都为0
    vector<int> v2(5);
 
    //元素个数为5，每个int元素都为3
    vector<int> v3(5, 3);
 
    //手动赋初值，共五个元素，元素值为指定的内容
    vector<int> v4{1, 2, 3, 4, 5};
}
```

## 操作

访问Vector中的任意元素或从末尾添加元素的时间复杂度是`O(1)`，而查找特定值的元素所处的位置或是在Vector中插入元素则是线性时间复杂度，即`O(n)`。

### 增加元素

#### 下标插入

Vector是动态数组，是支持随机访问的，也就是直接用下标取值。

但是如果是直接用下标赋值，当下标超出了容器的超出了容量，会无法插入，而且此时是不会报错。

```c++
/*
 * 声明了一个对象，初始是两个元素，容量为2
 * 当直接修改下标没有超过容量，会直接修改元素
 * 当直接修改下标查过了容量，会没有变化，因为容器内不存在超过容量的元素，被认为是无效操作。
 * */
void add1(){
    vector<int> demo{1, 2};
    demo[1]=3;//{1,3}
    demo[10]=3;//{1,3}
}
```

所以建议使用自带的插入函数

#### insert插入

允许多个元素的插入，使用迭代器指定位置。

**注意：是在迭代器 `pos` 位置之前插入！**

注意：因为需要调用类的构造函数和移动构造函数，所以较慢，但是适用性很棒！

```c++
void add2(){
    vector<int> demo{1, 2};
    //在第一个元素后面插入3
    demo.insert(demo.begin() + 1, 3);//{1,3,2}
 
    //在末尾插入2个5
    demo.insert(demo.end(), 2, 5);//{1,3,2,5,5}
 
    //插入其他容器的部分序列
    set<int> setTemp{7, 8, 9};
    demo.insert(demo.end(), ++setTemp.begin(), --setTemp.end()); //{1,3,2,5,5,8}
 
    //插入初始化列表
    demo.insert(demo.end(), {10, 11}); //{1,3,2,5,5,7,8,9,10,11}
 
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
}
```

#### emplace插入

注意是**每次只能插入一个**，而且是只有构造函数，效率高！

```
void add3() {
    vector<int> demo{1, 2};
 
    demo.emplace(demo.begin(), 3);//{3,1,2}
 
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
}
```

#### push_back插入

vector底层是用**数组**实现的，每次执行`push_back`操作，在底层实现时，是会判断当前元素的个数是否等于容量大小，如果没有就直接插入，否则就要扩容了。

```c++
void add4() {
    vector<int> demo{1, 2};
 
    demo.push_back(3);//{3,1,2}
 
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
}
```

换句话说，扩容时是要重新分配大小的，先`free`掉原来的存储空间，后重新`malloc`。非常耗费时间

```c++
void
vector<_Tp, _Allocator>::push_back(const_reference __x)
{
    if (this->__end_ != this->__end_cap())
    {
        __construct_one_at_end(__x);
    }
    else
        __push_back_slow_path(__x);
}
```



### 遍历元素

#### 下标遍历1

```c++
/*
 * 直接for循环，用下标取元素即可
 * */
void search1() {
    vector<int> demo{1, 2};
 
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
}
```

#### 下标遍历2

```c++
/*
 * 直接用下标取值，超过容量会报错
 * */
void search3() {
    vector<int> demo{1, 2};
 
    cout <<demo.at(1);
    // cout <<demo.at(1);// 会报错
    cout << endl;
}
```

#### 迭代器遍历

```c++
/*
 * 直接用迭代器，注意const_iterator还是iterator
 * */
void search2() {
    vector<int> demo{1, 2};
 
    // 如果参数为const vector<int> 需要用const_iterator
    // vector<int>::const_iterator iter=v.begin();
    for (vector<int>::iterator it = demo.begin(); it != demo.end(); ++it) {
        cout << (*it) << " ";
    }
    cout << endl;
}
```



### 删除元素

```c++

/*
 * 删除有两种方式，
 * clear一个是直接清空
 * erase是删除指定迭代器范围内的数字
 * pop_back是删除最后一个
 * */
void del() {
    vector<int> demo{1, 2, 3, 4, 5};
    //清空
    demo.clear();//{}
    if (demo.empty()) {//判断Vector为空则返回true
        demo.insert(demo.end(), {6, 7, 8, 9, 10, 11});//{ 6, 7, 8, 9, 10, 11 }
 
        //删除第一个元素
        demo.erase(demo.begin());//{7, 8, 9, 10, 11 }
 
        //删除前三个
        demo.erase(demo.begin(), demo.begin() + 3); //{ 10, 11 }
 
        //删除最后一个
        demo.pop_back();//{10}
    }
 
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
}
```



## 原理

### 内存扩展方式

当调用插入函数，却发现空间不够的时候，会进行扩容：

1. 寻找原来的`capacity`的两倍空间；

2. 将原数据复制过去；

3. 释放原空间三部曲。


每次配置新空间时都有留下一些数据空间，可以保证常数的时间复杂度

### 三大特性

- 严格的线性顺序排序。
- 支持动态内存分配

- 支持随机访问元素，并操供了在尾部添加和删除操作


### size和capacity的区别

`size`则代表了对象内元素的个数

`capacity`代表了能够存放多少个元素的阀值。

一旦超过阀值`capacity`，容器会花费大量时间重新配置内部的存储器，并导致`vector`元素相关的所有`reference`、`pointers`、`iterator`都会失效。



# priority_queue

**一种优先级队列，经常当作大顶堆来使用！**

**优先级高的元素先出队列！**

## 定义

其模板声明带有三个参数，`priority_queue<Type, Container, Functional>`, 其中`Type`为数据类型，`Container`为保存数据的容器，`Functional`为元素比较方式。`Container`必须是用数组实现的容器，比如 `vector`, `deque`. `STL`里面默认用的是`vector`. 比较方式默认用`operator<` , 所以如果把后面两个参数缺省的话，优先队列就是大顶堆，队头元素最大。

```c++
struct Node {
    int index1;
    int index2;
    int sum;
 
    // i1+i2=sum
    Node(int i1, int i2, int sum) {
        this->index1 = i1;
        this->index2 = i2;
        this->sum = sum;
    }
 
    //从小到大排列，会形成大顶堆
    friend bool operator<(Node n1, Node n2) {
        return n1.sum < n2.sum;
    }
};
 
priority_queue<Node> m;//默认大顶堆辅助
```



- `priority_queue()`，默认按照从小到大排列。所以`top()`返回的是最大值而不是最小值！
- 使用`greater<>`后，数据从大到小排列，`top()`返回的就是最小值而不是最大值！



如果使用了第三个参数，那第二个参数不能省，用作保存数据的容器！

```c++
priority_queue<int,vector<int> , greater<>> pq;
```



这里再提一嘴，`greater<int>`与`greater<int>()` 的区别，这要根据函数原型要求参数是**函数对象类型**还是要求参数是**结构类型**。`greater<int>` 对应于结构的类型，`greater< int>()`对应于没有参数且返回类型更大的函数的类型。比如`multimap`中使用不带括号的，`sort`使用带括号的。



## 基本操作

- empty( )  //判断一个队列是否为空

- pop( )  //删除队顶元素

- push( )  //加入一个元素

- size( )  //返回优先队列中拥有的元素个数

- top( )  //返回优先队列的队顶元素

优先队列的时间复杂度为`O(logn)`，`n`为队列中元素的个数，其存取都需要时间



# map

`map` 是**有序**的键值对容器，元素的**键是唯一的，值允许重复**。用比较函数 `Compare` 排序键。搜索、移除和插入操作拥有对数复杂度，即`O(logn)`。 底层实现为红黑树。



## 成员函数

| 元素访问                |                                                  |
| :---------------------- | :----------------------------------------------- |
| at                      | 用索引访问指定的元素，同时进行越界检查           |
| [operator]              | 用索引访问或插入指定的元素                       |
| 迭代器                  |                                                  |
| begin和cbegin(C++11)    | 返回指向起始的迭代器                             |
| end和cend(C++11)        | 返回指向末尾的迭代器                             |
| rbegin和crbegin(C++11)  | 返回指向起始的逆向迭代器                         |
| rend和crend(C++11)      | 返回指向末尾的逆向迭代器                         |
| 容量                    |                                                  |
| empty                   | 检查容器是否为空                                 |
| size                    | 返回容纳的元素数                                 |
| max_size                | 返回可容纳的最大元素数                           |
| 修改器                  |                                                  |
| clear                   | 清除内容                                         |
| insert                  | 插入元素或结点 (C++17 起)                        |
| insert_or_assign(C++17) | 插入元素，或若键已存在则赋值给当前元素           |
| emplace(C++11)          | 原位构造元素                                     |
| emplace_hint(C++11)     | 使用提示原位构造元素                             |
| try_emplace(C++17)      | 若键不存在则原位插入，若键存在则不做任何事       |
| erase                   | 擦除元素                                         |
| swap                    | 交换内容                                         |
| extract(C++17)          | 从另一容器释出结点                               |
| merge(C++17)            | 从另一容器接合结点                               |
| 查找                    |                                                  |
| count                   | 返回匹配特定键的元素数量                         |
| find                    | 寻找带有特定键的元素                             |
| contains(C++20)         | 检查容器是否含有带特定键的元素                   |
| equal_range             | 返回匹配特定键的元素范围                         |
| lower_bound             | 返回指向首个不小于给定键的元素的迭代器           |
| upper_bound             | 返回指向首个大于给定键的元素的迭代器             |
| 观察器                  |                                                  |
| key_comp                | 返回用于比较键的函数                             |
| value_comp              | 返回用于在`value_type`类型的对象中比较键的函数。 |



## 增加元素

总共有三种插入方式。

```c++
void add1() {
    map<int, string> m(
            {
                    {1, "A"},
                    {3, "C"},
                    {2, "B"}
            }
    );
 
    // 当索引是不存在的值，成功插入；当索引已经存在，则不进行操作
    //调用make_pair函数模板，好处是构造对象不需要参数，用起来更方便
    m.insert(pair<int, string>(24, "Z"));
    m.insert(map<int, string>::value_type(23, "Y"));
    m.insert(make_pair(1, "Z"));
 
    // 索引是原先没有的，直接插入；索引已经存在直接修改
    m[22] = "X";
    m[3] = "X";
 
    // 当索引是不存在的值，成功插入；当索引已经存在，则不进行操作
    m.emplace(pair<int, string>(21, "W"));
    m.emplace(pair<int, string>(1, "W"));
 
    map<int, string>::iterator iter;
    for (iter = m.begin(); iter != m.end(); iter++) {
        cout << iter->first << ' ' << iter->second << endl;
    }
}
// 1 A
// 2 B
// 3 X
// 21 W
// 22 X
// 23 Y
// 24 Z
```



以上三种用法，虽然都可以实现数据的插入，但是它们是有区别的:

用`insert`函数和`emplace`函数插入数据，在数据的插入上涉及到集合的唯一性这个概念，即当`map`中有这个关键字时，`insert`操作是插入数据不了的。

用索引`[]`方式就不同了，它可以覆盖对应的值。



## 遍历元素

强烈建议**使用迭代器**遍历集合！

```c++
void search1() {
    map<int, string> m(
            {
                    {1, "A"},
                    {3, "C"},
                    {2, "B"}
            }
    );
 
    map<int, string>::iterator iter;
    for (iter = m.begin(); iter != m.end(); iter++) {
        cout << iter->first << ' ' << iter->second << endl;
    }
}
// 1 A
// 2 B
// 3 C
```

下面介绍一个反面例子，看看直接使用索引去遍历而产生的结果。

```c++
void search2() {
    map<int, string> m(
            {
                    {1, "A"},
                    {3, "C"},
                    {5, "B"}
            }
    );
    cout << "遍历前元素的个数：" << m.size() << endl;
    for (int i = 0; i < m.size(); i++) {
        cout << i << ' ' << m[i] << endl;
    }
    cout << "遍历后元素的个数：" << m.size();
 
}
//遍历前元素的个数：3
// 0
// 1 A
// 2
// 3 C
// 4
// 5 B
// 遍历后元素的个数：6
```

很明显，因为没有判定是否存在而是直接无脑使用，原意是遍历一遍集合，结果却是修改了集合！



## 删除元素

### 直接删除元素

可以清空，也可以用迭代器删除指定范围元素或者单个元素。

但是在遍历的时候要注意，使用迭代器删除元素后，迭代器可能会变成类似野指针的存在！

```c++
/*
 * 删除有两种方式，
 * clear是直接清空
 * erase是删除指定迭代器范围内的数字
 * 也可以用来删除指定的单个元素
 * */
void del1() {
    map<int, string> m(
            {
                    {1, "A"},
                    {2, "B"},
                    {3, "C"}
            }
    );
    //清空
    m.clear();//{}
    if (m.empty()) {//判断Vector为空则返回true
        m.insert(pair<int, string>(4, "D"));
        m.insert(pair<int, string>(5, "E"));
        m.insert(pair<int, string>(6, "F"));
 
        //用迭代器删除单个元素，注意指针被删除后就失效了
        map<int, string>::iterator iter = m.begin();
        m.erase(iter);//所剩元素{5,E},{6,F}，此时的iter仍然是{4,D}
        cout << "错误的迭代器内容：" << iter->first << ' ' << iter->second << endl;
 
        //删除一个范围， 只保留最后一个
        m.erase(m.begin(), ++m.end()); //{6,F}
 
        //通过关键字索引的数据存在就删除，并返回1；如果关键字索引的数据不存在就不操作，并返回0
        m.erase(2);
 
    }
    map<int, string>::iterator iter;
    for (iter = m.begin(); iter != m.end(); iter++) {
        cout << iter->first << ' ' << iter->second << endl;
    }
}
```



### 遍历集合并删除元素

如果想要遍历整个`map`，并删除所有满足指定数值的应该如下：

```c++
/*
 * 遍历集合以删除指定条件的元素
 * */
void del2() {
    map<int, string> m(
            {
                    {1, "A"},
                    {2, "B"},
                    {3, "C"}
            }
    );
    map<int, string>::iterator iter;
 
    // 删除元素后，期望iter指针是继续指向{3,C}的，
    // 但是经过iter++后，竟然又到了上一个元素！
    // 很明显，删除元素后的迭代器变成了类似野指针的存在！
    // for (iter = m.begin(); iter != m.end(); iter++) {
    //     if (iter->first == 2 || iter->second == "B") {
    //         m.erase(iter);
    //     }
    //     cout << iter->first << ' ' << iter->second << endl;
    // }
    //结果是：
    // 1 A
    // 2 B
    // 1 A
    // 3 C
 
    // 正确做法应该是先复制出来一个临时迭代器
    // 接着将原来的迭代器后移一位指向正常的元素
    // 最后用临时迭代器删除指定元素！
    // 第二步和第三步不能反了，否则也会影响到原来正常的迭代器！
    for (iter = m.begin(); iter != m.end();) {
        if (iter->first == 2) {
            map<int, string>::iterator iterTemp = iter;
            ++iter;
            m.erase(iterTemp);
        } else {
            cout << iter->first << ' ' << iter->second << endl;
            ++iter;
        }
    }
    // 结果是
    // 1 A
    // 3 C
}
```



用迭代器删除元素，先是断言确定迭代器不是尾迭代器，接着将当前迭代器复制到一个新对象，最后返回的就是这个新的迭代器对象。调用`_M_erase_aux`方法删除迭代器指向的元素，并且节点数目减一。



```c++
void _M_erase_aux(const_iterator __position) {
    _Link_type __y =
            static_cast<_Link_type>(_Rb_tree_rebalance_for_erase
                    (const_cast<_Base_ptr>(__position._M_node),
                     this->_M_impl._M_header));
    _M_drop_node(__y);
    --_M_impl._M_node_count;
}
```



## 查找函数

### count统计元素个数

`count`函数是用来统计一个元素在当前容器内的个数。由于`Map`的特性，所以只能返回1或者0

```c++
/*
 * 用count函数寻找元素，
 * */
void find1(set<int> s) {
    if (s.count(4) == 1) {
        cout << "元素4存在" << endl;
    }
    if (s.count(8) == 0) {
        cout << "元素8不存在";
    }
}
```



追查源码，我发现他是用的`find`方法，将结果跟尾迭代器比较，如果不等于尾迭代器就是找到了，返回1；反之就是没找到，返回0。

```c++
find(const _Key &__k) const {
    const_iterator __j = _M_lower_bound(_M_begin(), _M_end(), __k);
    return (__j == end()
            || _M_impl._M_key_compare(__k,
                                      _S_key(__j._M_node))) ? end() : __j;
}
```



### find获取元素迭代器

```c++
/*
 * 用find函数寻找元素，
 * */
void find2(set<int> s) {

    if (s.find(4) != s.end()) {
        cout << "元素4存在" << endl;
    } else {
        cout << "元素4不存在";
    }
    if (s.find(8) != s.end()) {
        cout << "元素8存在" << endl;
    } else {
        cout << "元素8不存在";
    }
}
```

而底层是调用的不带`const`标的`find`函数，函数体是一样的！而其中的核心逻辑就是用`_M_lower_bound`函数查找来确定位置。



## 比较函数

### key排序

`map`中默认就是使用`key`排序的，自动按照key的大小，增序存储，这也是作为key的类型必须能够进行 `<` 运算比

较的原因。

首先看一眼map模板的定义，重点看下第三个参数： `class Compare = less<Key> `

```c++
template < class Key， class T， class Compare = less<Key>，
           class Allocator = allocator<pair<const Key，T> > > class map;
```

与`less`相对的还有`greater`，都是STL里面的一个函数对象，那么什么是函数对象呢？

函数对象：即调用操作符的类，其对象常称为函数对象（function object），它们是行为类似函数的对象。表现出一个函数的特征，就是通过“对象名+(参数列表)”的方式使用一个 类，其实质是对`operator()`操作符的重载。

### value排序

逻辑上是先转为`vector`数组，接着将数组用指定的规则重新排序得到排序好的结果。至于是否用排序好的数组去转换为`map`对象则是看要求了。

```c++
bool Special(pair<string, int> a, pair<string, int> b) {
    return a.second < b.second;//从小到大排序
}

void specialCompare() {
    // 初始map集合
    map<string, int> m;
    m["a"] = 2;
    m["b"] = 3;
    m["c"] = 1;

    // 转为vector集合
    vector <pair<string, int>> demo(m.begin(), m.end());

    for (auto it = demo.begin(); it != demo.end(); ++it) {
        cout << (*it).first << " " << (*it).second << endl;
    }
    cout << endl;

    // 排序后查看效果
    sort(demo.begin(), demo.end(), Special);

    for (auto it = demo.begin(); it != demo.end(); ++it) {
        cout << (*it).first << " " << (*it).second << endl;
    }
    cout << endl;

    // 转换为新的map集合，区别就是前后类型反了。
    map<int, string> m2;
    for (vector < pair < string, int > > ::iterator it = demo.begin(); it != demo.end();
    ++it){
        m2[(*it).second] = (*it).first;
    }

    map<int, string>::iterator iter;
    for (iter = m2.begin(); iter != m2.end(); iter++) {
        cout << iter->first << ' ' << iter->second << endl;
    }
}
// a 2
// b 3
// c 1
//
// c 1
// a 2
// b 3
//
// 1 c
// 2 a
// 3 b
```



# set

集合(`Set`)是一种**元素唯一**的、包含**已排序**对象的数据容器。

`C++ STL`中标准关联容器`set`， `multiset`， `map`， `multimap`内部采用的就是一种非常高效的平衡检索二叉树：红黑树，也称为RB树(`Red-Black Tree`)。RB树的统计性能要好于一般平衡二叉树，所以被STL选择作为了关联容器的内部结构。

对于`map`和`set`这种关联容器来说，不需要做内存拷贝和内存移动。以节点的方式来存储，其节点结构和链表差不多。



博主当前的CPP版本为`c++14`，所以相关源码内容就是以这个版本的为准。

## 定义

```c++
#include<set>
using namespace std;
 
set<int> s1;//空对象
set<int> s2{3, 4, 2, 1};//列表清单,默认less递增 ,输出为{1,2,3,4}
set<int, greater<int> > s3{6, 5, 7, 8};//列表清单 ,输出为{8.7.6.5}
```

## 常规操作

支持正向和反向迭代器，但是**不支持随机迭代器访问**元素

> [C++中文在线手册](https://zh.cppreference.com/)：https://zh.cppreference.com/



| 成员方法         | 功能                                                         |
| ---------------- | ------------------------------------------------------------ |
| begin()          | 返回指向容器中第一个（注意，是已排好序的第一个）元素的双向迭代器。如果 set 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| end()            | 返回指向容器最后一个元素（注意，是已排好序的最后一个）所在位置后一个位置的双向迭代器，通常和 begin() 结合使用。如果 set 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| rbegin()         | 返回指向最后一个（注意，是已排好序的最后一个）元素的反向双向迭代器。如果 set 容器用 const 限定，则该方法返回的是 const 类型的反向双向迭代器。 |
| rend()           | 返回指向第一个（注意，是已排好序的第一个）元素所在位置前一个位置的反向双向迭代器。如果 set 容器用 const 限定，则该方法返回的是 const 类型的反向双向迭代器。 |
| cbegin()         | 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改容器内存储的元素值。 |
| cend()           | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改容器内存储的元素值。 |
| crbegin()        | 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改容器内存储的元素值。 |
| crend()          | 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改容器内存储的元素值。 |
| find(val)        | 在 set 容器中查找值为 val 的元素，如果成功找到，则返回指向该元素的双向迭代器；反之，则返回和 end() 方法一样的迭代器。另外，如果 set 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| lower_bound(val) | 返回一个指向当前 set 容器中第一个大于或等于 val 的元素的双向迭代器。如果 set 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| upper_bound(val) | 返回一个指向当前 set 容器中第一个大于 val 的元素的迭代器。如果 set 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| equal_range(val) | 该方法返回一个 pair 对象（包含 2 个双向迭代器），其中 pair.first 和 lower_bound() 方法的返回值等价，pair.second 和 upper_bound() 方法的返回值等价。也就是说，该方法将返回一个范围，该范围中包含的值为 val 的元素（set 容器中各个元素是唯一的，因此该范围最多包含一个元素）。 |
| empty()          | 若容器为空，则返回 true；否则 false。                        |
| size()           | 返回当前 set 容器中存有元素的个数。                          |
| max_size()       | 返回 set 容器所能容纳元素的最大个数，不同的操作系统，其返回值亦不相同。 |
| insert()         | 向 set 容器中插入元素。                                      |
| erase()          | 删除 set 容器中存储的元素。                                  |
| swap()           | 交换 2 个 set 容器中存储的所有元素。这意味着，操作的 2 个 set 容器的类型必须相同。 |
| clear()          | 清空 set 容器中所有的元素，即令 set 容器的 size() 为 0。     |
| emplace()        | 在当前 set 容器中的指定位置直接构造新元素。其效果和 insert() 一样，但效率更高。 |
| emplace_hint()   | 在本质上和 emplace() 在 set 容器中构造新元素的方式是一样的，不同之处在于，使用者必须为该方法提供一个指示新元素生成位置的迭代器，并作为该方法的第一个参数。 |
| count(val)       | 在当前 set 容器中，查找值为 val 的元素的个数，并返回。注意，由于 set 容器中各元素的值是唯一的，因此该函数的返回值最大为 1。 |

## 增加元素

### insert插入

**允许多个元素的插入**，使用迭代器指定位置。

```c++
/*
 * insert能插入多个，慢但是实用
 * */
void add1() {
    set<int> demo{1, 2};
    //在第一个元素后面插入3
    demo.insert(demo.begin()++, 3);//{1,2,3},结果遵循递增规则
 
    //直接插入元素,也是按照规则排列
    demo.insert(-1);//{-1,1,2,3}
    //C++11之后,可以用emplace_hint或者emplace替代
 
    //插入其他容器的部分序列
    vector<int> test{7, 8, 9};
    demo.insert(++test.begin(), --test.end()); //{-1,1,2,3,8}
 
    //插入初始化列表
    demo.insert({10, 11}); //{-1,1,2,3,8,10,11}
 
    set<int> s = demo;
    set<int>::iterator iter;
    for (iter = s.begin(); iter != s.end(); ++iter) {
        cout << *iter << " ";
    }
}
```



### emplace插入

注意是**每次只能插入一个**，而且是只有构造函数，效率高！

```c++
/*
 * emplace，只能插入一个
 * 而且如果元素已经存在，是不会再次插入的
 * */
void add2() {
    set<int> demo{1, 2};
 
    demo.emplace(4);//{1,2,4}
 
    demo.emplace(4);//还是{1,2,4}
 
    set<int> s = demo;
    set<int>::iterator iter;
    for (iter = s.begin(); iter != s.end(); ++iter) {
        cout << *iter << " ";
    }
}
```



## 遍历元素

```c++
/*
 * 直接用迭代器，注意const_iterator还是iterator
 * */
void search() {
    set<int> demo{1, 2};
 
    // 如果参数为const vector<int> 需要用const_iterator
    // vector<int>::const_iterator iter=v.begin();
 
    set<int> s = demo;
    set<int>::iterator iter;
    for (iter = s.begin(); iter != s.end(); ++iter) {
        cout << *iter << " ";
    }
}
```



## 删除元素

```c++
/*
 * 删除有两种方式，
 * clear是直接清空
 * erase是删除指定迭代器范围内的数字
 * 也可以用来删除指定的单个元素
 * */
void del() {
    set<int> demo{1, 2, 3, 4, 5};
    //清空
    demo.clear();//{}
    if (demo.empty()) {//判断Vector为空则返回true
        demo.insert({6, 7, 8, 9, 10, 11});//{ 6, 7, 8, 9, 10, 11 }
 
        //删除第一个元素
        demo.erase(demo.begin());//{7, 8, 9, 10, 11 }
        // 删除指定元素
        demo.erase(11);
        //删除前三个
        demo.erase(demo.begin(), next(demo.begin(), 3)); //{ 10, 11 }
        // 只保留最后一个
        demo.erase(demo.begin(), ++demo.end()); //{11}
    }
 
    set<int> s = demo;
    set<int>::iterator iter;
    for (iter = s.begin(); iter != s.end(); ++iter) {
        cout << *iter << " ";
    }
}
```



## 比较函数

### 元素为基本类型

#### 使用函数对象

当元素是基本类型，一般是变动规则为递增或者递减。

推荐使用自带的`less`或者`greater`函数比较。

```c++
// 基础类型，变动规则为递增或者递减
void BasicCompare() {
    set<int, less<int> > s2{6, 5, 7, 8};//小数靠前。递增,输出为{5,6,7,8}
    set<int, greater<int> > s3{6, 5, 7, 8};//大数靠前。递减,输出为{8,7,6,5}
 
    // 遍历查看内容
    set<int> s = s2;
    set<int>::iterator iter;
    for (iter = s.begin(); iter != s.end(); ++iter) {
        cout << *iter << " ";
    }
}
```

#### 类外重载括号

当元素是基本类型，变动规则后不能用自带的`less`或者`greater`函数比较，就只能新建一个结构体来描述规则。这种方式是**适用性最广**的。

```c++
// 基础类型，但是有特殊的比较规则
// 此处以递增递减为例
// 当然也可以指定为其他自定义类型
struct Special {
    bool operator()(const int &a, const int &b) {
        // return a > b;//等价greater<int>
        return a < b;//等价less<int>
    }
};
 
// 基础类型需要类外重载
void SpecialCompare() {
    set<int, Special> s2;
 
    s2.emplace(3);
    s2.emplace(2);
    s2.emplace(1);
    s2.emplace(4);
 
    // 遍历查看内容
    set<int, Special> s = s2;
    set<int>::iterator iter;
    for (iter = s.begin(); iter != s.end(); ++iter) {
        cout << *iter << " ";
    }
}
/*1 2 3 4 */
```



### 元素为自定义类型

当元素是自定义结构体，可以在类外重载括号`()`；也可以在类内重载小于号`<`，例子如下。

#### 类内重载小于号

```c++
// 自定义结构体
struct Info {
    string name;
    float score;

    Info() {
        name = "a";
        score = 60;
    }

    Info(string sname, float fscore) {
        name = sname;
        score = fscore;
    }

    // 类内重载方法
    bool operator<(const Info &a) const {
        /*名字递减；若名字相同，则分数递减*/
        if (a.name < name)
            return true;
        else if (a.name == name) {
            return a.score < score;
        } else
            return false;
    }
};

void InfoCompare() {
    pair<set<Info>::iterator, bool> result;//获取插入的结果
    set <Info> s;
    // 插入默认对象和指定对象
    s.insert(Info());
    s.insert(Info("a", 53));
    s.insert(Info("keen", 68));
    result = s.insert(Info("keen", 60));

    // 遍历查看内容
    for (auto iter = s.begin(); iter != s.end(); ++iter) {
        cout << iter->name << " " << iter->score << endl;
    }
    cout << "插入元素的信息：" << result.first->name << " " << result.first->score << endl;
    cout << "插入是否成功" << boolalpha << result.second << endl;

    // 再次插入相同元素
    result = s.insert(Info("keen", 60));
    cout << "插入元素的信息：" << result.first->name << " " << result.first->score << endl;
    cout << "插入是否成功" << boolalpha << result.second << endl;
}
/*
keen 68
keen 60
a 60
a 53
插入元素的信息：keen 60
插入是否成功true
插入元素的信息：keen 60
插入是否成功false
 */
```

#### 类外重载括号

```c++

#include <iostream>
#include <set>

using namespace std;

/*Student结构体*/
struct Student {
    string name;
    int age;
    string sex;

    Student() {}

    Student(const string &name, int age, const string &sex) : name(name), age(age), sex(sex) {}
};

/*
 * 为Student指定排序准则
 * 先比较名字；若名字相同,则比较年龄。小的返回true
 * 如果想要部分参数匹配，可以用正则表达式来规定
 * */
class StudentCompare {
public:
    bool operator()(const Student &a, const Student &b) const {
        if (a.name < b.name)
            return true;
        else if (a.name == b.name) {
            if (a.age < b.age)
                return true;
            else
                return false;
        } else
            return false;
    }
};

int main() {
    set<Student, StudentCompare> stuSet;

    stuSet.insert(Student("张三", 13, "male"));
    stuSet.insert(Student("李四", 23, "female"));

    /*构造一个用来查找的临时对象,可以看到,即使stuTemp与stu1实际上并不是同一个对象,
     *但当在set中查找时,仍能够查找到集合中的元素成功。
     *这是因为已定义的StudentCompare的缘故。
     */
    Student stuTemp;
    stuTemp.name = "张三";
    stuTemp.age = 13;

    set<Student, StudentCompare>::iterator iter;
    iter = stuSet.find(stuTemp);
    if (iter != stuSet.end()) {
        cout << (*iter).name << " " << (*iter).age << " " << (*iter).sex << endl;
    } else {
        cout << "Cannot fine the student!" << endl;
    }
    return 0;
}
```



## 查找函数

### count统计元素个数

`count`函数是用来统计一个元素在当前容器内的个数。由于`Set`的特性，所以只能返回1或者0。

```c++
/*
 * 用count函数寻找元素，
 * */
void find1(set<int> s ){
    if (s.count(4) == 1) {
        cout << "元素4存在"<<endl;
    }
    if (s.count(8) == 0) {
        cout << "元素8不存在";
    }
}
```

追查源码，我发现他是用的红黑树的`__count_unique`方法，从根节点开始比对，如果要查找的元素是比当前节点的数值要小就去左子树找，如果要查找的元素是比当前节点的数值要大就去右子树找，当匹配到节点值相等时，就返回1；当到最后的叶子结点仍然没有找到就返回0。

底层是使用的红黑树，所以查找起来还是很方便的。

### find获取元素迭代器

```c++
/*
 * 用find函数寻找元素，
 * */
void find2(set<int> s ){
 
    if (s.find(4)!= s.end() ) {
        cout << "元素4存在"<<endl;
    }else{
        cout << "元素4不存在";
    }
    if (s.find(8)!= s.end() ) {
        cout << "元素8存在"<<endl;
    }else{
        cout << "元素8不存在";
    }
}
```

追查源码，我发现他是用的红黑树的`find`方法，从根节点开始比对，如果要查找的元素是比当前节点的数值要大就去右子树找；如果查找的元素比当前节点的数值要**小于等于**匹配到节点值时，就**赋值一次结果节点并去左子树找**。一直到最后所查找的节点为空再结束。最终返回结果节点。

暂时看来，两个方法的底层实现逻辑是相似的，都是用平衡二叉树的方式去寻找节点。

区别是`count`返回1或0来标明元素是否存在；`find`函数是返回指针可以方便下一步修改或者取用。



> C++ STL set容器完全攻略（超级详细）
>
> http://c.biancheng.net/view/7192.html



# AcWing 讲义

## vector

变长数组、倍增的思想

- ​    size()  返回元素个数
- ​    empty()  返回是否为空
- ​    clear()  清空
- ​    front()/back()
- ​    push_back()/pop_back()
- ​    begin()/end()
- ​    []
- ​    支持比较运算，按字典序

## pair<int, int>

- ​    first, 第一个元素
- ​    second, 第二个元素
- ​    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

## string

字符串

- ​    size()/length()  返回字符串长度
- ​    empty()
- ​    clear()
- ​    substr(起始下标，(子串长度))  返回子串
- ​    c_str()  返回字符串所在字符数组的起始地址

## queue

队列

- ​    size()
- ​    empty()
- ​    push()  向队尾插入一个元素
- ​    front()  返回队头元素
- ​    back()  返回队尾元素
- ​    pop()  弹出队头元素

## priority_queue

优先队列、默认是大根堆

- ​    size()

- ​    empty()

- ​    push()  插入一个元素

- ​    top()  返回堆顶元素

- ​    pop()  弹出堆顶元素

- ​    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

  ​    或者也可以直接插入一个数的负值，负数由大到小排序，相当于正数由小到大排序

## stack

栈

- ​    size()
- ​    empty()
- ​    push()  向栈顶插入一个元素
- ​    top()  返回栈顶元素
- ​    pop()  弹出栈顶元素

## deque

双端队列

- ​    size()
- ​    empty()
- ​    clear()
- ​    front()/back()
- ​    push_back()/pop_back()
- ​    push_front()/pop_front()
- ​    begin()/end()
- ​    []

## set  map  multiset  multimap

基于平衡二叉树（红黑树），动态维护有序序列

- ​    size()
- ​    empty()
- ​    clear()
- ​    begin()/end()
- ​    ++, -- 返回前驱和后继，时间复杂度 O(logn)

### set/multiset

所有操作的时间复杂度都是  logn

- ​    insert()  插入一个数
- ​    find()  查找一个数     如果不存在的话返回 end() 迭代器
- ​    count()  返回某一个数的个数
- ​    erase()
  1. 输入是一个数x，删除所有x     时间复杂度：O(k + logn)   k 是 x 的个数
  2. 输入一个迭代器，删除这个迭代器
- ​    lower_bound()/upper_bound()
  - lower_bound(x)  返回大于等于x的最小的数的迭代器
  - upper_bound(x)  返回大于x的最小的数的迭代器

### map/multimap

map 存储的是一个映射关系，所以存储结构是一个 pair

- ​    insert()  插入的数是一个pair
- ​    erase()  输入的参数是pair或者迭代器
- ​    find()
- ​    []  注意仅仅是 map 支持这个操作，multimap 不支持此操作。 时间复杂度是：O(logn)
- ​    lower_bound()/upper_bound()

## unordered_set  unordered_map  unordered_multiset  unordered_multimap

哈希表和上面类似，增删改查的时间复杂度是 O(1)

因为是无序的，所以不支持 lower_bound()/upper_bound()，也不支持迭代器的++，--

## bitset

圧位

```c++
bitset<10000> s;
```

支持以下操作符：   

​    ~、 &、 |、 ^
​    >>, <<
​    ==, !=
​    []

- count()  返回有多少个1

- any()  判断是否至少有一个1
- none()  判断是否全为0
- set()  把所有位置成1
- set(k, v)  将第k位变成v
- reset()  把所有位变成0
- flip()  等价于~
- flip(k) 把第k位取反