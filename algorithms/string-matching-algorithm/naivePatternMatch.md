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
