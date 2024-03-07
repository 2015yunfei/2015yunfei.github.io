## AcWing 2065. 整除序列

https://www.acwing.com/problem/content/2067/

有一个序列，序列的第一个数是 *n*，后面的每个数是前一个数整除 2，请输出这个序列中值为正数的项。

#### 输入格式

输入一行包含一个整数 *n*

#### 输出格式

输出一行，包含多个整数，相邻的整数之间用一个空格分隔，表示答案。

#### 数据范围

1≤*n*≤10^18^



#### 输入样例：

```
20
```

#### 输出样例：

```
20 10 5 2 1
```

```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    long long int n;
    cin >> n;
    while (n > 0) {
        printf("%lld ", n);
        n /= 2;
    }
    return 0;
}
```



## AcWing 2066. 解码

https://www.acwing.com/problem/content/2068/

小明有一串很长的英文字母，可能包含大写和小写。

在这串字母中，有很多连续的是重复的。

小明想了一个办法将这串字母表达得更短：将连续的几个相同字母写成字母 + 出现次数的形式。

例如，连续的 5个 *a*，即aaaaa，小明可以简写成 a5（也可能简写成 a4a、aa*3*a等）。

对于这个例子：HHHellllloo，小明可以简写成 *H*3el5*o*2为了方便表达，小明不会将连续的超过 9 个相同的字符写成简写的形式。现在给出简写后的字符串，请帮助小明还原成原来的串。

#### 输入格式

输入一行包含一个字符串。

#### 输出格式

输出一个字符串，表示还原后的串。

#### 数据范围

输入字符串由大小写英文字母和数字组成，长度不超过 100
请注意原来的串长度可能超过 100

#### 输入样例：

```
H3el5o2
```

#### 输出样例：

```
HHHellllloo
```

自己写得，从第二位考虑第一位，实际代码比较复杂

```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    string s;
    cin >> s;
    if (s.length() == 1) cout << s;
    else {
        for (int i = 1; i < s.length(); i++) {
            if (s[i] > '1' && s[i] <= '9') {
                int k = s[i] - '0';
                for (int j = 0; j < k; ++j) {
                    printf("%c", s[i - 1]);
                }
                i++;
            } else {
                printf("%c", s[i - 1]);
            }
        }
        if (s[s.length() - 1] < '0' || s[s.length() - 1] > '9') {
            printf("%c", s[s.length() - 1]);
        }
    }
    return 0;
}
```

y总代码，从第一位考虑第二位，实际代码简洁很多

```c++
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    string s, res;
    cin >> s;

    for (int i = 0; i < s.size(); i++) {
        if (i + 1 < s.size() && s[i + 1] <= '9') {
            int k = s[i + 1] - '0';
            while (k--) res += s[i];
            i++;
        } else {
            res += s[i];
        }
    }

    cout << res << endl;
    return 0;
}
```



## AcWing 2067. 走方格

https://www.acwing.com/problem/content/2069/

在平面上有一些二维的点阵。这些点的编号就像二维数组的编号一样，从上到下依次为第 1至第 *n* 行，从左到右依次为第 1 至第 *m*列，每一个点可以用行号和列号来表示。现在有个人站在第 1行第 1 列，要走到第 n 行第m列。只能向右或者向下走。注意，如果行号和列数都是偶数，不能走入这一格中。问有多少种方案。

#### 输入格式

输入一行包含两个整数 *n*,*m*

#### 输出格式

输出一个整数，表示答案。

#### 数据范围

1≤*n*,*m*≤30

#### 输入样例1：

```
3 4
```

#### 输出样例1：

```
2
```

#### 输入样例2：

```
6 6
```

#### 输出样例2：

```
0
```

```c++
#include<bits/stdc++.h>

using namespace std;
const int N = 40;
int n, m;
int f[N][N];

int main() {
    cin >> n >> m;
    f[0][1] = 1;
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if (i % 2 != 0 || j % 2 != 0) {
                f[i][j] = f[i - 1][j] + f[i][j - 1];
            }
        }
    }
    cout << f[n][m];
    return 0;
}
```

如果用DFS的话，十个测试数据只能过九个。

所以要追求完美的话，应该是使用递推，不过要注意初始化的位置。



## AcWing 2068. 整数拼接

https://www.acwing.com/problem/content/2070/

给定一个长度为 *n* 的数组 *A*1,*A*2,⋅⋅⋅,*An*。你可以从中选出两个数 *Ai*和 *Aj*(*i* 不等于 *j*)，然后将 *Ai* 和 *Aj* 一前一后拼成一个新的整数。例如 12 和 345 可以拼成 12345 或 34512。注意交换 *Ai* 和 *Aj* 的顺序总是被视为 2 种拼法，即便是 *Ai*=*Aj* 时。请你计算有多少种拼法满足拼出的整数是 *K* 的倍数。

#### 输入格式

第一行包含 2个整数 *n* 和 *K*

第二行包含 *n* 个整数 *A*1,*A*2,⋅⋅⋅,*An*

#### 输出格式

一个整数代表答案。

#### 数据范围

1≤*n*≤10^5^
1≤*K*≤10^5^,
1≤*Ai*≤10^9^

#### 输入样例：

```
4 2
1 2 3 4
```

#### 输出样例：

```
6
```

```c++
#include <bits/stdc++.h>

using namespace std;
typedef long long int LL;
const int N = 1e5 + 10;
int n, k;
int a[N], s[11][N];

int main() {
    cin >> n >> k;
    for (int i = 0; i < n; ++i) {
        scanf("%d", &a[i]);
    }
    for (int i = 0; i < n; ++i) {
        LL t = a[i] % k;
        for (int j = 0; j < 11; ++j) {
            s[j][t]++;
            t = t * 10 % k;
        }
    }
    LL ans = 0;
    for (int i = 0; i < n; ++i) {
        int t = a[i] % k;
        t = (k - t) % k;
        int len = to_string(a[i]).size();
        ans += s[len][t];
        LL r = a[i];
        while (len--) {
            r = r * 10 % k;
        }
        if (r % k == t)ans--;
    }
    cout << ans;
    return 0;
}
```



## AcWing 2069. 网络分析

https://www.acwing.com/problem/content/2071/

小明正在做一个网络实验。

他设置了 *n* 台电脑，称为节点，用于收发和存储数据。

初始时，所有节点都是独立的，不存在任何连接。

小明可以通过网线将两个节点连接起来，连接后两个节点就可以互相通信了。

两个节点如果存在网线连接，称为相邻。

小明有时会测试当时的网络，他会在某个节点发送一条信息，信息会发送到每个相邻的节点，之后这些节点又会转发到自己相邻的节点，直到所有直接或间接相邻的节点都收到了信息。

所有发送和接收的节点都会将信息存储下来。

一条信息只存储一次。

给出小明连接和测试的过程，请计算出每个节点存储信息的大小。

#### 输入格式

输入的第一行包含两个整数 *n*,*m*，分别表示节点数量和操作数量。节点从 1至 *n* 编号。接下来 *m* 行，每行三个整数，表示一个操作。如果操作为 `1 a b`，表示将节点 *a* 和节点 *b* 通过网线连接起来。当 a = b 时，表示连接了一个自环，对网络没有实质影响。如果操作为 `2 p t`，表示在节点 *p* 上发送一条大小为 *t* 的信息。

#### 输出格式

输出一行，包含 *n* 个整数，相邻整数之间用一个空格分割，依次表示进行完上述操作后节点 1 至节点 *n* 上存储信息的大小。

#### 数据范围

1≤*n*≤10000
1≤*m*≤105
1≤*t*≤100



#### 输入样例1：

```
4 8
1 1 2
2 1 10
2 3 5
1 4 1
2 2 2
1 1 2
1 2 4
2 2 1
```

#### 输出样例1：

```
13 13 5 3
```

```c++
#include <bits/stdc++.h>

using namespace std;
const int N = 1e4 + 10;
int n, m;
int p[N], cnt[N];

int find(int x) {
    while (p[x] != x) x = p[x];
    return p[x];
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) p[i] = i;
    while (m--) {
        int op, a, b;
        scanf("%d%d%d", &op, &a, &b);
        if (op == 1) {
            a = find(a);
            b = find(b);
            if (a != b) {
                p[a] = b;
                cnt[a] -= cnt[b];
            }
        } else {
            a = find(a);
            cnt[a] += b;
        }
    }
    for (int i = 1; i <= n; ++i) {
        int j = i;
        int sum = cnt[j];
        while (p[j] != j) {
            j = p[j];
            sum += cnt[j];
        }
        printf("%d ", sum);
    }
    return 0;
}
```

自己写得代码中，find函数没有路径压缩，运行速度2400ms，比较慢

```c++
#include <iostream>

using namespace std;

const int N = 10010;

int n, m;
int p[N], d[N];

int find(int x) {
    if (p[x] == x || p[p[x]] == p[x]) return p[x];
    int r = find(p[x]);
    d[x] += d[p[x]];
    p[x] = r;
    return r;
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) p[i] = i;
    while (m--) {
        int t, a, b;
        scanf("%d%d%d", &t, &a, &b);
        if (t == 1) {
            a = find(a), b = find(b);
            if (a != b) {
                d[a] -= d[b];
                p[a] = b;
            }
        } else {
            a = find(a);
            d[a] += b;
        }
    }

    for (int i = 1; i <= n; i++)
        if (i == find(i)) printf("%d ", d[i]);
        else printf("%d ", d[i] + d[find(i)]);
    puts("");

    return 0;
}
```

y总代码，运行速度非常快，60ms

![image-20230309144002752](./库/image-20230309144002752.png)



## AcWing 2875. 超级胶水

https://www.acwing.com/problem/content/2878/

小明有 *n* 颗石子，按顺序摆成一排。

他准备用胶水将这些石子粘在一起。

每颗石子有自己的重量，如果将两颗石子粘在一起，将合并成一颗新的石子，重量是这两颗石子的重量之和。

为了保证石子粘贴牢固，粘贴两颗石子所需要的胶水与两颗石子的重量乘积成正比，本题不考虑物理单位，认为所需要的胶水在数值上等于两颗石子重量的乘积。

每次合并，小明只能合并位置相邻的两颗石子，并将合并出的新石子放在原来的位置。

现在，小明想用最少的胶水将所有石子粘在一起，请帮助小明计算最少需要多少胶水。

#### 输入格式

输入的第一行包含一个整数 *n* ，表示初始时的石子数量。

第二行包含 *n* 个整数 *w*1,*w*2,…,*wn*，依次表示每颗石子的重量。

#### 输出格式

一个整数表示答案。

#### 数据范围

1≤*n*≤10^5^
1≤*wi*≤1000

#### 输入样例1：

```
3
3 4 5
```

#### 输出样例1：

```
47
```

#### 输入样例2：

```
8
1 5 2 6 3 7 4 8
```

#### 输出样例2：

```
546
```

```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
    long long int x = 0, ans = 0, res = 0;
    int n;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        scanf("%lld", &x);
        ans += x * res;
        res += x;
    }
    cout << ans;
    return 0;
}
```

这题本来我以为是个区间DP题,但是根据数据范围1e5发现连数组都开不了，由此此题可能是个找规律的题
可以先把需要合并的数设为x1,x2,x3。将x1,x2,x3合并有两种方法.
一；先将x1,x2合并，再合并x3
二；先将x2,x3合并，再合并x1

第一种需要的胶水数为;(x1x2)+(x1+x2)x3 ----> x1x2+x1x3+x2x3 –提取公因式—>(x2x3)+(x2+x3)x1==第二种所需要的胶水数;
第二种需要的胶水数为;(x2x3)+(x2+x3)x1;

由此发现当有3堆石子时无论如何合并，其结果一样，由此类推，对于n堆石子，可以把n堆分为3大堆，再把每大堆再划分为3大堆。所以无论有多少堆，其结果都一样，即合并顺序不会影响使用的胶水数。

另：

当n=2时：ab
当n=3时：ab+(a+b)c=ac+(a+c)b=bc+(b+c)a=ab+ac + bc
当n=4时：ab+(a+b)c+(a+b+c)d=ac+(a+c)b+(a+c+b)d=.....=ab+ac+ad+ bc+bd +cd
以此类推 每个数分别与后面的所有数相乘之和即可

也就是等于

当n=2时: ab
当n=3时：ab+ac+bc=ab+(a+b)c
当n=4时：ab+ac+ad+bc+bd+cd=ab+(a+b)c+(a+b+c)*d