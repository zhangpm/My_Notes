---
title: Dynamic Programming
---
# 动态规划
## 1. Fibonacci Sequence
1. 1,1,2,3,4,5,8,13
2. fib(n)=fib(n-1)+fib(n-2) (n!=1 or 2)
3. 递归关系：overlap sub-problem
4. 计算时会生成树，复杂度为O($2^n$),出现很多重复计算
5. Capture.JPG
6. 出现重复计算，即重叠子问题（overlap sub-problem），保存已经算过的值
7. 代码：
- 1) 简单递归 
```python
def fab(n):
    # 终止条件 边界
    if n <= 2:
        return 1
    else:
        # 最优子结构 状态转移公式
        return fab(n - 1) + fab(n - 2)
```
- 2) 去掉重复运算，记录结果 
```python
#定义字典
dict_fab = {}

def fab_2(n):
    # 终止条件 边界
    if n <= 2:
        return 1
    elif dict_fab.get(n):
    #get返回指定键的值，如果值不在字典中返回default值None
        print('*')
        return dict_fab.get(n)
    else:
        # 最优子结构 状态转移公式
        # 字典添加键值对：字典[键]=值
        dict_fab[n] = fab_2(n - 1) + fab_2(n - 2)
        return dict_fab[n]
```
- 3) 动态规划算法：由前向后推
```python
# 最终优化 动态规划  （大问题化成若干相同类型的子问题 然后一个个解决子问题）
def fab_3(n):
    # 由前往后推
    a = 1
    b = 1
    if n <= 2:
        print('fab({})={}'.format(n, b))
        return 1
    for i in range(n - 2):
        print(a, b)
        a, b = b, a + b
    print('fab({})={}'.format(n, b))
    return b
```
例题：
1. 1,2,4,1,7,8,3 数字选择，不能相邻，使求和最大
解：
上式给入数组v[0,,,n],
转移公式：
$$ OPT(n)=max\left\{
\begin{aligned}
OPT(prev(n))+v(n), 选 \\
OPT(n-1)， 不选 \\
\end{aligned}
\right.$$
说明：
* 选第n个，则计算prev(n)个（不选n后可选的数）的最优解加上n的值；
* 不选第n个，则计算n-1个的最优解
递归出口：
上式子最后是opt(1)=max(v(1),opt(0));opt(0)=1

代码：**动态规划一般为非递归形式，在递归（从后向前）的基础上改为非递归（从前向后）**
递归法：
```python
v=[1,2,4,1,7,8,3]

def rec_opt(v,i):
    if i==0:
        return v[0]
    elif i==1:
        return max(v[1],rec_opt(v,0))
    else:
        return max(rec_opt(v,i-2)+v[i],rec_opt(v,i-1))


print(rec_opt(v,6))
```
非递归：自底向上
```python

lenth=len(v)
opt=[0]*len(v)
opt[0]=v[0]
opt[1]=max(v[1],rec_opt(v,0))
def dp_opt(v):
    for i in range(2,lenth):
        A=opt[i-2]+v[i]
        B=opt[i-1]
        opt[i]= max(A, B)
    return opt[-1]
print(dp_opt(v))
```

