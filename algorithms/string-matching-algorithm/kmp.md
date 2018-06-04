### kmp algorithm

#### principle

#### pseudo code

#### code with java


#### test case


#### time & space complexity.

---

#### 算法本质

    这个算法针对的是子串有对称属性,如果有对称属性,那么要向前查找是否有可以再次匹配的内容
    next[]描述的是子串的对称程度,程度越高,值越大,可能出现再匹配的机会就更大
    (不是中心对称,而是中心字符块对称,比如不是abccba,而是abcabc这种对称)
    
    失配后move多少呢? 失配位j之前已经匹配的字符长度-next[j]值; 
    如果对称程度越高,next[j]值越大,即移动的位越小因为可能再次出现匹配的情况.
    如果没有对称,则移动最大,即直接跳过

#### 数学归纳法

    0.初始条件next[0]=0
    1.假设next[j]=k成立
    2.推到next[j+1]=?
        2.1 如果pattern[k]==pattern[j] 推出 next[j+1]=k+1
        2.1 如果pattern[k]!=pattern[j] 递归 k=next[k] 直到k=0

#### 图形化描述

   [KMP算法的Next数组详解](http://www.cnblogs.com/tangzhengyue/p/4315393.html)
   ![](https://images0.cnblogs.com/blog2015/326320/201503/051048038058339.png)

#### 代码实现

    private static int[] next(char[] pattern)
    {
        int[] nextArr = new int[pattern.length];
        nextArr[0] = 0;

        int j, k;//q:模版字符串下标；k:最大前后缀长度

        for (j = 1,k = 0; j < pattern.length; ++j)//for循环，从第二个字符开始，依次计算每一个字符对应的next值
        {
            while(k > 0 && pattern[j] != pattern[k])//递归的求出P[0]···P[q]的最大的相同的前后缀长度k
                k = nextArr[k-1];          //不理解没关系看下面的分析，这个while循环是整段代码的精髓所在，确实不好理解
            if (pattern[j] == pattern[k])//如果相等，那么最大相同前后缀长度加1
            {
                k++;
            }
            nextArr[j] = k;
        }
        return nextArr;
    }



















