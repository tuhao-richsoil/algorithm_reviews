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

1）模板(用单调队列优化):

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

