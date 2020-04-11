# 闫式DP分析法

- **核心：从集合角度来分析DP问题**

- **DP问题的核心思想：求有限集中的最值**
- DP分析一般分为两个阶段
  - 化零为整的过程(状态表示f(i))，把某些具有相似状态的元素化为一个子集然后用某一个状态来表示。(一般从两个角度来分析)
    - **f(i)表示的是哪个集合(所有满足条件1..条件2...条件3...的选法的集合，每个条件都对应了一个维度)**
    - f(i)一般存的是一个值，它和f(i)有什么关系(一般是Max/Min/Count)
  - 化整为零的过程(状态计算)
    - 看f(i)表示的状态是什么--->把f(i)划分成若干个子集来求(**划分的依据一般时寻找最后一个不同点**)
      - 子集满足几个条件(不重复，不遗漏)(求数量时必须保证不重复，求MAX可以重复)

## 常见DP问题模型

- 选择问题(背包模型)
- 序列问题(最长XX子序列模型)
- 状态压缩DP模型
- 状态积模型
- 树形DP模型
- 区间DP模型
- 单?堆优化模型
- 斜率优化模型

## 01背包问题

```一件物品只有选和不选两种情况```

**题目:**

- **有*N*件物品和一个容量是*V*的背包。每件物品只能使用一次。**
- 第*i*件物品的体积是*Vi*，价值是*Wi*
- 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大
- 输出最大价值

**输入格式**

- 第一行两个整数，*N*,*V* 用空格隔开。分别表示物品数量和背包容积
- 接下来又*N*行，每行两个整数*Vi*，*Wi*用空格隔开，分别表示第*i*件物品的体积和价值

**输出格式**

输出一个整数，表示最大价值

**数据范围**
$$
0<N，V\leq1000
$$

$$
0<v_i,w_i\leq1000
$$

**输入样例**

```java
4 5
1 2
2 4
3 4
4 5
```

**输出样例**

```java
8
```

### 闫式分析

- 这个题目是一个有限集合求最值的问题
- 状态表示(化零为整)(选择问题的状态表示)
  - 第一维一般只考虑前i个物品，后面几维一般都是**限制**
    - f(i,j)
      - 集合:所有只考虑前i个物品并且总体积不超过j的选法的集合
      - 属性:集合中每一个方案的最大价值Max
      - f(N,v)就是最后的答案
- 状态计算(化整为零)
  - 不同点就是选不选第i个物品

$$
f(i,j) = Max{(f(i-1,j),f(i-1,j-v_i)+w_i)}
$$

```java
package backpack01;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main{
    public static void main(String[] args) throws Exception{
        // Scanner sc = new Scanner(System.in);
        // int item = sc.nextInt();
        // int limit = sc.nextInt();
        // int[] volume = new int[item+1];
        // int[] worth = new int[item+1];
        // for (int i = 1; i <= item; i++) {
        //     volume[i] = sc.nextInt();
        //     worth[i] = sc.nextInt();
        // }
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] s = br.readLine().split(" ");
        String[] s1;
        int item = Integer.parseInt(s[0]);
        int limit = Integer.parseInt(s[1]);
        int[] volume = new int[item+1];
        int[] worth = new int[item+1];
        for(int i = 1 ; i<=item;i++){
            s1 = br.readLine().split(" ");
            volume[i] = Integer.parseInt(s1[0]);
            worth[i] = Integer.parseInt(s1[1]);
        }
        br.close();
        int[][] dp = new int[item+1][limit+1];
        for(int i = 1; i <= item;i++){
            for(int j = 0;j<=limit;j++){
                dp[i][j] = dp[i-1][j];
                if(j >= volume[i]){
                    dp[i][j] = Math.max(dp[i][j],dp[i-1][j-volume[i]]+worth[i]);
                }
            }
        }
        System.out.print(dp[item][limit]);
    }
}
```

- **优化**

  - **dp问题的所有优化都是对代码的等价优化**

  - 时间不能再优化了

  - 观察状态转移方程，发现第i个状态只与第i-1个状态有关，可以使用滚动数组(每次只需要保持上一次(i-1)的结果即可)

  - 第二维要么是0要么是v_i，所以可以只用一个数组从大到小循环

  - ```java
    for(j = V;i>=vi;i--)
    ```

- 优化之后

$$
f(j) = Max((f(j),f(j-v_i)+w_i))
$$

```java
int[] dp = new int[limit+1];
for(int i = 1; i <= item ;i++){
    for(int j = limit ; j >= volume[i] ;j--){
        dp[j] = Math.max(dp[j],dp[j-volume[i]]+worth[i]);
    }
}
System.out.print(dp[limit]);
```



## 完全背包问题

```一件物品可以被无限次选择```

**题目:**

- **有*N*件物品和一个容量是*V*的背包。每件物品可以无限次使用。**
- 第*i*件物品的体积是*Vi*，价值是*Wi*
- 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大
- 输出最大价值

**输入格式**

- 第一行两个整数，*N*,*V* 用空格隔开。分别表示物品数量和背包容积
- 接下来又*N*行，每行两个整数*Vi*，*Wi*用空格隔开，分别表示第*i*件物品的体积和价值

**输出格式**

输出一个整数，表示最大价值

**数据范围**
$$
0<N，V\leq1000
$$

$$
0<v_i,w_i\leq1000
$$

**输入样例**

```java
4 5
1 2
2 4
3 4
4 5
```

**输出样例**

```java
10
```

### 闫式分析

- 这个题目是一个有限集合求最值的问题
- 状态表示(化零为整)(选择问题的状态表示)
  - 第一维一般只考虑前i个物品，后面几维一般都是**限制**
    - f(i,j)
      - 集合:所有只考虑前i个物品并且总体积不超过j的选法的集合
      - 属性:集合中每一个方案的最大价值Max
      - f(N,V)就是最后的答案
- 状态计算(化整为零)
  - 第i个物品不选
  - 第i个物品选1次
  - ...
  - 第i个物品选k次

$$
f(i,j) = max(f[i-1][j],f[i-1][j-v_i]+w_i,f[i-1][j-2v_i]+2w_i+.....)
$$

$$
f(i,j-v_i) = max(f[i-1][j-v_i],f[i-1][j-2v_i]+w_i+f[i-1][j-3_vi]+2w_i+......)
$$

$$
f(i,j) = max(f[i-1][j],f[i][j-v_i]+w_i)(与01背包的状态计算不同的是第二项f[i])
$$

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] strs1 = br.readLine().split(" ");
        int item = Integer.parseInt(strs1[0]);
        int limit = Integer.parseInt(strs1[1]);
        
        int[] volume = new int[item+1];
        int[] worth = new int[item+1];
        
        String[] strs2;
        for(int i = 1; i < item+1 ;i++){
            strs2 = br.readLine().split(" ");
            volume[i] = Integer.parseInt(strs2[0]);
            worth[i] = Integer.parseInt(strs2[1]);
        }
        
        int[][] dp = new int[item+1][limit+1];
        for(int i = 1 ; i <= item ;i++){
            for(int j = 0 ; j <= limit ;j++){
                dp[i][j] = dp[i-1][j];
                if(j >= volume[i]){
                    dp[i][j] = Math.max(dp[i][j],dp[i][j-volume[i]] + worth[i]);
                }
            }
        }
        System.out.println(dp[item][limit]);
    } 
}
```

- **优化**

```java
    int[] dp = new int[limit+1];
    for(int i = 1 ; i <= item ;i++){
        for(int j = volume[i] ; j <= limit ;j++){
            dp[j] = dp[j]
            //dp[i][j] = dp[i-1][j];
            //右边的dp[j]实际上是本次还未更新的dp[j]也就是dp[i-1][j],所以等价    
            if(j >= volume[i]){
                dp[j] = Math.max(dp[j],dp[j-volume[i]]+worth[i]);
                //dp[i][j] = Math.max(dp[i][j],dp[i][j-volume[i]] + worth[i]);
                //volume[i] > 0 所以j-volume[i] < j 所以dp[j-volume[i]]是已经算出来了的也就是dp[i][j-volume[i]]所以完全等价
                //不要像上面那样想，画出二维解空间来看是i还是i-1
            }
        }
    }
```


## 石子合并(区间DP问题)

```区间dp问题```

**题目:**

- 设有N堆石子排成一排，其编号为1，2，3，...，N
- 每堆石子有一定的质量，可以用一个整数来描述，现在要将这N堆石子合并为一堆
- 每次**只能合并相邻**的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆相邻的石子将和新堆相邻。
- 合并时由于选择的顺序不同，合并的总代价也不相同
- 例如有4堆石子分别为1 3 5 2，我们可以先合并1、2堆，代价为4，得到4 5 2，又合并1，2堆，代价为9，得到9 2，再合并得到11，总代价为4+9+11=24
- 问题是：找出一种合理的方法，使总的代价最小，输出最小代价

**输入格式**

- 第一行一个数表示石子的堆数N
- 第二行N个数，表示每堆石子的质量(均不超过1000)

**输出格式**

输出一个整数，表示最大价值

**数据范围**
$$
0<N\leq300
$$

**输入样例**

```java
4
1 3 5 2
```

**输出样例**

```java
22
```

### 闫式分析

- 只能合成相邻两堆
- 最后一定时把某两堆合成一堆
  - 左大堆一定是左边连续的一部分
  - 右大堆一定是右边连续的一部分

- 状态表示f(i,j)
  - **表示的集合：所有将[i,j]合并成一堆的方案的集合**
  - 属性：Min

- 状态计算
  - 最后一次合并一定是左部分和有部分合并
  - 所以就以这个分界点作为子集划分依据

$$
f(i,j) = f(i,k) + f(k+1,j) + s[j] - s[i]
$$

- 区间dp问题
  - 先枚举区间长度
  - 再枚举区间左端点
  - 再枚举区间右端点

```java
package MergeStone;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * 石子合并(区间DP问题)
 */
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String s1 = br.readLine();
        int N = Integer.parseInt(s1);
        String[] s2 = br.readLine().split(" ");

        int[] s= new int[N+1];    //前缀和
        for(int i = 1; i <= N ;i++){
            s[i] = Integer.parseInt(s2[i-1]);
            s[i] += s[i-1];
        }
        int[][] dp = new int[N+1][N+1];
        for(int len = 2 ; len <= N; len++){
            for(int i = 1 ; i + len - 1 <= N; i++){
                int j = i + len -1;
                dp[i][j] = Integer.MAX_VALUE;
                for(int k = i ; k < j ;k++){
                    dp[i][j] = Math.min(dp[i][j],dp[i][k]+dp[k+1][j]+ s[j] - s[i-1]);
                }
            }
        }
        System.out.println(dp[1][N]);
    }
}
```

## 最长公共子序列

```序列dp问题```

**题目:**

- 给定两个长度分别为N和M的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少

**输入格式**

- 第一行包含两个整数N和M
- 第二行包含一个长度为N的字符串，表示字符串A
- 第三行包含一个长度为M的字符串，表示字符串B

**输出格式**

输出一个整数，表示最大长度

**数据范围**
$$
0<N\leq1000
$$

**输入样例**

```java
4 5
acbd
abedc
```

**输出样例**

```java
3
```

### 闫式分析

- 是一个有限集求最值的问题
- **状态表示f(i,j)**
  - 集合：所有字符串A[1~i]与B[1~j]的公共子序列的集合
  - 属性：Max

- **状态计算**
- 00(A[i]和B[j]都不包含)
  - a[1~i-1]和b[1~j-1]的公共子序列的最大长度

$$
f(i,j) = f(i-1)(j-1)
$$

- 01(A[i]不包含，B[j]包含)
  - f(i,j) = f(i-1,j)
    - f(i-1,j)一定不包含a[i],但是也可能不包含b[j]
    - **求最大值的时候是可以重复的，所以可以用f(i-1,j)来计算**

$$
f(i,j) = f(i-1)(j)
$$



- 10(A[i]包含，B[i]不包含)
  - 同上

$$
f(i,j) = f(i,j-1)
$$



- 11(A[i]包含，B[i]包含)
  - 只有当A[i] == B[j]时才有这种情况

$$
f(i,j) = f(i-1)(j-1) + 1
$$

- 总状态计算
  - 注意(0,0)的所有方案包含在了(0,1)中，所以只算后面三个就可以了

$$
f(i,j) = Max(f(i-1)(j),f(i)(j-1),f(i-1)(j-1)+1)
$$

```java
package LongestCommonSubSequence;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * 最长子序列
 */
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] s1 = br.readLine().split(" ");
        int N = Integer.parseInt(s1[0]);
        int M= Integer.parseInt(s1[1]);
        String a = br.readLine();
        String b = br.readLine();
        br.close();
        char[] cA = a.toCharArray();
        char[] cB = b.toCharArray();
        char[] aC = new char[N+1];
        char[] bC = new char[M+1];
        for(int i = 1 ; i <= N ;i++){
            aC[i] = cA[i-1];
        }
        for(int i = 1 ; i <= M ;i++){
            bC[i] = cB[i-1];
        }

        int[][] dp = new int[N+1][M+1];
        for(int i = 1 ; i<=N ;i++){
            for(int j = 1 ; j<=M;j++){
                dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
                if(aC[i] == bC[j]){
                    dp[i][j] = Math.max(dp[i][j],dp[i-1][j-1]+1);
                }
            }
        }
        System.out.println(dp[N][M]);
    }
}
```

- 思考：输入应该怎么优化