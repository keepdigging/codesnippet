### Naive Pattern Matching

#### With Java

/**
 * mainChars : target string
 * subChars : pattern string
 * return : return match index
 **/
public int match(char[] mainChars, char[] subChars)
{
    if(subChars.length > mainChars.length)
    {
        return -1;
    }
    int i, j = 0;
    for(; i < mainChars.length; i++)
    {
        if(mainChars[i] == subChars[j])
        {
            j++;
        }
        else
        {
            j=0;
            i=i-j+1;
        }
    }
    //handle results.
    if()
    {
        return ;
    }
    else
    {
        return -1;
    }
}
