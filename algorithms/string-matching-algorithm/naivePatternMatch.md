### Naive Pattern Matching

#### principle

#### pseudo code

#### Code With Java

    /**
    * Naive String Searching Algothrim.
    * Match only once.
    **/
    public int naiveMatchOnce(char[] text, char[] pattern)
    {
        if(pattern.length > text.length)
        {
            return -1;
        }
        int i, j = 0;
        while(i < text.length && j < pattern.length)
        {
            if(text[i] == pattern[j])
            {
                i++;
                j++;
            }
            else
            {
                j = 0;
                i = i - j + 1;
            }
        }
        if(j >= pattern.length)
        {
            return i - j;
        }
        else
        {
            return -1;
        }

    }

#### test case.

    Assert.assertEqual(-1, naiveMatchOnce({'a','b','c','d'}, {'a','b','c','d','e'}));

    Assert.assertEqual(0, naiveMatchOnce({'a','b','c'}, {'a','b','c'}));
    Assert.assertEqual(-1, naiveMatchOnce({'a','b','c'}, {'a','c','b'}));

    Assert.assertEqual(0, naiveMatchOnce({'a','b','c','d'}, {'a','b','c'}));
    Assert.assertEqual(1, naiveMatchOnce({'a','b','c','d'}, {'b','c'}));
    Assert.assertEqual(2, naiveMatchOnce({'a','b','c','d'}, {'c','d'}));
    Assert.assertEqual(3, naiveMatchOnce({'a','b','c','d'}, {'d'}));

    Assert.assertEqual(-1, naiveMatchOnce({'a','b','c','d'}, {'a','b','d'}));
    Assert.assertEqual(-1, naiveMatchOnce({'a','b','c','d'}, {'a','c','d'}));
    Assert.assertEqual(-1, naiveMatchOnce({'a','b','c','d'}, {'b','d'}));
    Assert.assertEqual(-1, naiveMatchOnce({'a','b','c','d'}, {'a','d','d'}));

#### time & space complexity

    text: abcabcabcabcabcd (n length)
    pattern: abcd          (m length)

    worst situation: (n-m+1)*m
    best situation: 1*m

#### 工程实践

    JDK8 String.indexOf(String pattern)
    /**
     * Code shared by String and StringBuffer to do searches. The
     * source is the character array being searched, and the target
     * is the string being searched for.
     *
     * @param   source       the characters being searched.
     * @param   sourceOffset offset of the source string.
     * @param   sourceCount  count of the source string.
     * @param   target       the characters being searched for.
     * @param   targetOffset offset of the target string.
     * @param   targetCount  count of the target string.
     * @param   fromIndex    the index to begin searching from.
     */
    static int indexOf(char[] source, int sourceOffset, int sourceCount,
            char[] target, int targetOffset, int targetCount,
            int fromIndex) {
        if (fromIndex >= sourceCount) {
            return (targetCount == 0 ? sourceCount : -1);
        }
        if (fromIndex < 0) {
            fromIndex = 0;
        }
        if (targetCount == 0) {
            return fromIndex;
        }

        char first = target[targetOffset];
        int max = sourceOffset + (sourceCount - targetCount);

        for (int i = sourceOffset + fromIndex; i <= max; i++) {
            /* Look for first character. */
            if (source[i] != first) {
                while (++i <= max && source[i] != first);
            }

            /* Found first character, now look at the rest of v2 */
            if (i <= max) {
                int j = i + 1;
                int end = j + targetCount - 1;
                for (int k = targetOffset + 1; j < end && source[j]
                        == target[k]; j++, k++);

                if (j == end) {
                    /* Found whole string. */
                    return i - sourceOffset;
                }
            }
        }
        return -1;
    }

#### 细微差异




#### why not choose kmp or bm?







