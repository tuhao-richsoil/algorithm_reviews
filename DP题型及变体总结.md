# **DP题型及变体总结**

## **一、数字三角形**

### **1. 常规的三角和矩阵转移**

易错点：

1） 1018的边界处理问题，状态转移之前进行边界判断：

```c++
for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= n; ++j)
        {
            if(i > 1)   f[i][j] = min(f[i][j] , f[i - 1][j] + g[i][j]);
            if(j > 1)   f[i][j] = min(f[i][j] , f[i][j - 1] + g[i][j]);
        }
```



## **二、最长上升子序列**

## **三、背包问题**

### **1. 01背包**

1) 模板：

```c++
for(int i = 1; i <= n; ++i)
    {
        for(int j = t; j >= w[i]; --j)
        {
            f[j] = max(f[j] , f[ j - w[i] ]+ v[i]);
        }
    }
```

### 2. 完全背包

1）模板:

```c++
for(int i = 1; i <= n; ++i)
    {
        for(int j = w[i]; j <= m; ++j)
        {
            f[j] = max(f[j - v[i]] , f[ j - w[i] ]+ v[i]);
        }
    }
```



### 3. 分组背包

1）模板1(用单调队列优化):

思想: 单调队列管理j - v , j - 2*v ,..., j - s*v的最大值

难点1: **max(f[k] , g[q[hh]] + (k - q[hh]) / v * w)**

即计算f[j - a * v] + a * w

难点2: **while( hh <= tt && g[q[tt]] - (q[tt] - r) / v * w <= g[k] - (k - r) / v * w ) --tt;//r是基点**

即 j  - a * v 距离 j  - s * v有多少v

```c++
for(int i = 0; i < n; ++i)
    {
        int w , v, s;
        scanf("%d%d%d" , &v , &w , &s);

        for(int r = 0; r < v; ++r)
        {
            hh = 0 , tt = -1;
            memcpy(g , f , sizeof(f));
            for(int k = r; k <= m; k += v)
            {
                if(hh <= tt && q[hh] < k - s * v) ++hh;
                if(hh <= tt) f[k] = max(f[k] , g[q[hh]] + (k - q[hh]) / v * w);   //更新答案
                while( hh <= tt && g[q[tt]] - (q[tt] - r) / v * w <= g[k] - (k - r) / v * w ) --tt;//r是基点
                q[++tt] = k;
            }
        }
    }

```

2) 模板2(二进制优化)

思想:将每一种物品打包,每一包分别有1,2,4,...,s - 2^(k - 1)个，然后对每个包进行01背包求解，即完成了对改组物品的求解。

```c++
//分组
for(int i = 1; i <= n; ++i)
	{
		int a , b , s;
		cin>>a>>b>>s; 
		int k = 1;
		while(k <=  s)
		{
			cnt++;
			v[cnt] = k *  a;
			w[cnt] = k * b;
			s -= k;
			k *= 2;
		}
		if(s > 0)
		{
			cnt++;
			v[cnt] = s * a;
			w[cnt] = s * b;
		}
		
	}
	
	n = cnt;
	//做01背包问题
	for(int i = 1; i <= n; ++i)
		for(int j = m; j >= v[i]; --j)
			f[j] = max(f[j] , f[j - v[i]] + w[i]);
```

### 4. 混合背包(前三种之和)

1) 思想:由于每种物品的选择只依靠前一种物品是什么，所以当遇到一种物品时，将其分类，然后按照对应的背包问题进行状态计算即可，注意这里的分组背包可以使用二进制优化法,而且在进行01背包计算时要考虑到01背包是特殊的分组背包，而分组背包在打包是要记得乘上每个包的物品数量。

2) 模板:

```c++
for(int i = 1; i <= n; ++i)
    {
        int a , b , c;
        scanf("%d%d%d" , &a , &b , &c);
        if(c == 0)
        {
            for(int j = a; j <= m; ++j)
                f[j] = max(f[j] , f[j - a] + b);
        }
        else
        {
            if(c == -1) c = 1;
            for(int k = 1; k <= c; k *= 2)
            {
                for(int j = m; j >= a * k; --j)
                {
                    f[j] = max(f[j] , f[j - k * a] + k * b);
                }
                c -= k;
            }
            if(c)   
                for(int j = m; j >= a * c; --j)
                     f[j] = max(f[j] , f[j - c * a] + c * b);
        }
    }
```

### 5. 二维费用问题

1) 注解: 这不是一种专门的背包类型，而是与具体背包问题具体结合的问题，详见acwing.8

2) 状态表示分类及其对应的初始化方法:

<img src="D:\截图\Snipaste_2020-11-01_21-40-48.png" style="zoom:75%;" />

3) 注意:**情况(3)的状态由于是下界，所以允许为负数，但是负数下界必须写为0。**

​			 **至于不存在的转态初始化为正负无穷，要视题目所求集合属性决定。**

