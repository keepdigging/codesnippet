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
    
   [失配后移动多少位的数学证明](http://www.cnblogs.com/javaminer/p/3420213.html)

#### 数学归纳法

    0.初始条件next[0]=0
    1.假设next[j]=k成立
    2.推到next[j+1]=?
        2.1 如果pattern[k]==pattern[j] 推出 next[j+1]=k+1
        2.1 如果pattern[k]!=pattern[j] 递归 k=next[k] 直到k=0

#### 图形化描述

   [KMP算法的Next数组详解](http://www.cnblogs.com/tangzhengyue/p/4315393.html)
   ![](https://images0.cnblogs.com/blog2015/326320/201503/051048038058339.png)
   [概念解释](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)

#### 代码实现

    private int[] next(char[] pattern)
    {
        int[] nextArray = new int[pattern.length];
        nextArray[0] = 0;

        int k = 0;

        for (int j = 1; j<pattern.length; j++)
        {
            while (pattern[j] != pattern[k] && k > 0)//递归找相等的
            {
                k = nextArray[k-1];
            }
            if(pattern[j] == pattern[k])
            {
                k++;
            }
            nextArray[j] = k;
        }

        return nextArray;
    }

#### next[] test case.

    System.out.println(Arrays.toString(new KMP().next("abcd".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcda".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdab".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdabc".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdabcd".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdabcdg".toCharArray()))); 这里最后一个g就是没有找到
    System.out.println(Arrays.toString(new KMP().next("abcdabcda".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdabcdab".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdabcdabc".toCharArray())));
    System.out.println(Arrays.toString(new KMP().next("abcdabcdabcd".toCharArray())));
    
    输出: 
    [0, 0, 0, 0]
    [0, 0, 0, 0, 1]
    [0, 0, 0, 0, 1, 2]
    [0, 0, 0, 0, 1, 2, 3]
    [0, 0, 0, 0, 1, 2, 3, 4]
    [0, 0, 0, 0, 1, 2, 3, 4, 0] 所以g得next[]值为0 递归找到的
    [0, 0, 0, 0, 1, 2, 3, 4, 5]
    [0, 0, 0, 0, 1, 2, 3, 4, 5, 6]
    [0, 0, 0, 0, 1, 2, 3, 4, 5, 6, 7]
    [0, 0, 0, 0, 1, 2, 3, 4, 5, 6, 7, 8]


---

#### 另一种解释

    a.当前一个字符的对称程度为0的时候,只要将当前字符与子串第一个字符进行比较,这个很好理解啊,前面都是0,说明都不对称了
    如果多加了一个字符,要对称的话最多是当前的和第一个对称,比如agcta这个里面t的是0,那么后面的a的对称程度只需要看它是不是等于第一个字符a了

    b.按照这个推理,我们就可以总结一个规律,不仅前面是0呀,如果前面一个字符的next值是1,那么我们就把当前字符与子串第二个字符进行比较
    因为前面的是1,说明前面的字符已经和第一个相等了,如果这个又与第二个相等了，说明对称程度就是2了,有两个字符对称了,比如上面agctag
    倒数第二个a的next是1,说明它和第一个a对称了,接着我们就把最后一个g与第二个g比较,又相等,自然对称成都就累加了,就是2了

    c.当然不可能会那么顺利让我们一直对称下去,如果遇到下一个不相等了,那么说明不能继承前面的对称性了
    这种情况只能说明没有那么多对称了,但是不能说明一点对称性都没有,所以遇到这种情况就要重新来考虑,这个也是难点所在

    d.回头来找对称性

    这里已经不能继承前面了,但是还是找对称串嘛,最愚蠢的做法大不了写一个子函数,查找这个字符串的最大对称程度,怎么写方法很多吧
    比如查找出所有的当前字符串,然后向前走,看是否一直相等,最后走到子串开头,当然这个是最蠢的,我们一般看到的KMP都是优化过的

    因为这个串是有规律的,在这里依然用上面表中一段来举个例子,位置i=0到14如下,我加的括号只是用来说明问题：
    (a g c t a g c )( a g c t a g c) t
    我们可以看到这段,最后这个t之前的对称程度分别是：1,2,3,4,5,6,7,倒数第二个c往前看有7个字符对称,所以对称为7
    但是到最后这个t就没有继承前面的对称程度next值,所以这个t的对称性就要重新来求

    这里首要要申明几个事实
    1.t 如果要存在对称性,那么对称程度肯定比前面这个c 的对称程度小,所以要找个更小的对称,这个不用解释了吧,如果大那么t就继承前面的对称性了
    2.要找更小的对称,必然在对称内部还存在子对称,而且这个t必须紧接着在子对称之后

---

[KMP算法的前缀next数组最通俗的解释](https://www.cnblogs.com/ganhang-acm/p/4060508.html)

![](http://hi.csdn.net/attachment/201108/29/0_1314610552Nj5Q.gif)
![](http://hi.csdn.net/attachment/201108/29/0_1314610574Rjbs.gif)

---

#### Linux中kmp实现

    https://github.com/torvalds/linux/blob/b3a3a9c441e2c8f6b6760de9331023a7906a4ac6/lib/ts_kmp.c

#### 为了好看,简化版本

    void kmp_init(const char *patn, int len, int *next)
    {
       int i, j;
       next[0] = 0;
       for (i = 1, j = 0; i < len; i++) 
       {
           while (j > 0 && patn[j] != patn[i])
               j = next[j - 1];
           if (patn[j] == patn[i])
               j++;
           next[i] = j;
       }
    }

    int kmp_find(const char *text, int text_len, const char *patn, int patn_len, int *next)
    {
       int i, j;
       for (i = 0, j = 0; i < text_len; i ++ ) 
       {
           while (j > 0 && text[i] != patn[j])
               j = next[j-1];
           if (text[i] == patn[j])
               j++;
           if (j == patn_len)
               return i + 1 - patn_len;
       }
       return -1;
    }

#### 真实版本

    static unsigned int kmp_find(struct ts_config *conf, struct ts_state *state)
    {
        struct ts_kmp *kmp = ts_config_priv(conf);
        unsigned int i, q = 0, text_len, consumed = state->offset;
        const u8 *text;
        const int icase = conf->flags & TS_IGNORECASE;
    
        for (;;) {
            text_len = conf->get_next_block(consumed, &text, conf, state);
    
            if (unlikely(text_len == 0))
                break;
    
            for (i = 0; i < text_len; i++) {
                while (q > 0 && kmp->pattern[q] != (icase ? toupper(text[i]) : text[i]))
                    q = kmp->prefix_tbl[q - 1];
                if (kmp->pattern[q] == (icase ? toupper(text[i]) : text[i]))
                    q++;
                if (unlikely(q == kmp->pattern_len)) {
                    state->offset = consumed + i + 1;
                    return state->offset - kmp->pattern_len;
                }
            }
            consumed += text_len;
        }
        return UINT_MAX;
    }
    
    /**
     * 计算next数组
     **/
    static inline void compute_prefix_tbl(const u8 *pattern, unsigned int len, unsigned int *prefix_tbl, int flags)
    {
        unsigned int k, q;
        const u8 icase = flags & TS_IGNORECASE;
    
        for (k = 0, q = 1; q < len; q++) {
            while (k > 0 && (icase ? toupper(pattern[k]) : pattern[k]) != (icase ? toupper(pattern[q]) : pattern[q]))
                k = prefix_tbl[k-1];
            if ((icase ? toupper(pattern[k]) : pattern[k]) == (icase ? toupper(pattern[q]) : pattern[q]))
                k++;
            prefix_tbl[q] = k;
        }
    }

     *   Implements a linear-time string-matching algorithm due to Knuth,
     *   Morris, and Pratt [1]. Their algorithm avoids the explicit
     *   computation of the transition function DELTA altogether. Its
     *   matching time is O(n), for n being length(text), using just an
     *   auxiliary function PI[1..m], for m being length(pattern),
     *   precomputed from the pattern in time O(m). The array PI allows
     *   the transition function DELTA to be computed efficiently
     *   "on the fly" as needed. Roughly speaking, for any state
     *   "q" = 0,1,...,m and any character "a" in SIGMA, the value
     *   PI["q"] contains the information that is independent of "a" and
     *   is needed to compute DELTA("q", "a") [2]. Since the array PI
     *   has only m entries, whereas DELTA has O(m|SIGMA|) entries, we
     *   save a factor of |SIGMA| in the preprocessing time by computing
     *   PI rather than DELTA.
     *
     *   [1] Cormen, Leiserson, Rivest, Stein
     *       Introdcution to Algorithms, 2nd Edition, MIT Press
     *   [2] See finite automation theory

### [v_july_v版本](https://blog.csdn.net/v_july_v/article/details/7041827)
















### 实际项目中的常见算法

    http://www.infoq.com/cn/news/2013/11/Core-algorithms-deployed


### 完整算法

    /**
     * @param text
     * @param pattern
     * @return
     */
    public int kmp(char[] text, char[] pattern)
    {
        if(pattern.length > text.length)
        {
            return -1;
        }

        int j = 0;
        int[] next = nextArray(pattern);

        for (int i = 0; i<text.length; i++)
        {

            while (j > 0 && text[i] != pattern[j])
                j = next[j-1];

            if(text[i] == pattern[j])
            {
                j++;
            }
            if(j == pattern.length)
            {
                return i-j;
            }
        }

        return -1;
    }

    /**
     * @param pattern
     * @return
     */
    public int[] nextArray(char[] pattern)
    {
        int[] next = new int[pattern.length];
        next[0] = 0;

        int k = 0;
        for (int j = 1; j<pattern.length; j++)
        {
            while (k > 0 && pattern[j] != pattern[k])
            {
                k = next[k-1];
            }

            if(pattern[j] == pattern[k])
            {
                k++;
            }
            next[j] = k;
        }
        return next;
    }



























    










