## AcWing 3416. 时间显示

https://www.acwing.com/problem/content/3419/

小蓝要和朋友合作开发一个时间显示的网站。在服务器上，朋友已经获取了当前的时间，用一个整数表示，值为从 1970年 1 月 1 日 00:00:00到当前时刻经过的毫秒数。

现在，小蓝要在客户端显示出这个时间。

小蓝不用显示出年月日，只需要显示出时分秒即可，毫秒也不用显示，直接舍去即可。

给定一个用整数表示的时间，请将这个时间对应的时分秒输出。

#### 输入格式

输入一行包含一个整数，表示时间。

#### 输出格式

输出时分秒表示的当前时间，格式形如 `HH:MM:SS`，其中 `HH` 表示时，值为 0

 到 23，`MM` 表示分，值为 0 到 59，`SS` 表示秒，值为 0 到 59

时、分、秒不足两位时补前导 0

#### 数据范围

对于所有评测用例，给定的时间为不超过 10^18^ 的正整数。

#### 输入样例1：

```
46800999
```

#### 输出样例1：

```
13:00:00
```

#### 输入样例2：

```
1618708103123
```

#### 输出样例2：

```
01:08:23
```

```c++
#include <bits/stdc++.h>

using namespace std;
typedef long long int LL;

int main() {
    LL a;
    cin >> a;
    a /= 1000;
    LL mod = 24 * 3600;
    a = a % mod;
    int hh = a / 3600;
    a = a % 3600;
    int mm = a / 60;
    a = a % 60;
    int ss = a;
    if (ss == 60) {
        ss = 0;
        mm++;
    }
    if (mm >= 60) {
        mm -= 60;
        hh++;
    }
    printf("%02d:%02d:%02d", hh, mm, ss);
    return 0;
}
```



## AcWing 3417. 砝码称重

https://www.acwing.com/problem/content/3420/

你有一架天平和 *N* 个砝码，这 *N* 个砝码重量依次是 *W*1,*W*2,⋅⋅⋅,*WN*。

请你计算一共可以称出多少种不同的**正整数**重量？

注意砝码可以放在天平两边。

#### 输入格式

输入的第一行包含一个整数 *N*

第二行包含 *N* 个整数：*W*1,*W*2,*W*3,⋅⋅⋅,*WN*

#### 输出格式

输出一个整数代表答案。

#### 数据范围

对于 50% 的评测用例，1≤*N*≤15。
对于所有评测用例，1≤*N*≤100，*N* 个砝码总重不超过 10^5^

#### 输入样例：

```
3
1 4 6
```

#### 输出样例：

```
10
```

#### 样例解释

能称出的 10 种重量是：1、2、3、4、5、6、7、9、10、11

```c++
1 = 1；
2 = 6 − 4 (天平一边放 6，另一边放 4)；
3 = 4 − 1；
4 = 4；
5 = 6 − 1；
6 = 6；
7 = 1 + 6；
9 = 4 + 6 − 1；
10 = 4 + 6；
11 = 1 + 4 + 6。
```

![39421_a2e7f5a0a8-2](./库/39421_a2e7f5a0a8-2.png)



**y总代码**

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110, M = 200010, B = M / 2;

int n, m;
int w[N];
bool f[N][M];

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) scanf("%d", &w[i]), m += w[i];

    f[0][B] = true;
    for (int i = 1; i <= n; i++)
        for (int j = -m; j <= m; j++) {
            f[i][j + B] = f[i - 1][j + B];
            if (j - w[i] >= -m) f[i][j + B] |= f[i - 1][j - w[i] + B];
            if (j + w[i] <= m) f[i][j + B] |= f[i - 1][j + w[i] + B];
        }

    int res = 0;
    for (int j = 1; j <= m; j++)
        if (f[n][j + B])
            res++;
    printf("%d\n", res);
    return 0;
}
```

B 是偏移量

**题解区的代码**

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 110, M = 2e5 + 10;
int sum;
int n;
int w[N];
bool f[N][M];

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        scanf("%d", &w[i]);
        sum += w[i];
    }
    f[0][0] = true;
    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= sum; j++)
            f[i][j] = f[i - 1][j] || f[i - 1][j + w[i]] || f[i - 1][abs(j - w[i])];
    int ans = 0;
    for (int i = 1; i <= sum; i++)
        if (f[n][i]) ans++;
    cout << ans;
    return 0;
}
```

由实际含义可以，`f[i][j]=f[i][-j]` 因为可以通过镜像的操作而得到，故而`f[i][j-w]=f[i][w-j]=f[i][abs(j-w)]`。就避免了数组下标为负数的情况

