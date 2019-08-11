# Algorithm
> https://leetcode.com/problems/number-of-1-bits/description/

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while(n != 0 ){
            n= n & (n-1);
            count++;
        }
        return count;
    }
}
```

这个题目的大意是： 给定一个无符号的整数， 返回其用二进制表式的时候的1的个数。

这里用一个trick， 可以轻松求出。 就是n & (n - 1) 可以消除 n 最后的一个1的原理。

为什么能消除最后一个1， 其实也比较简单，大家自己想一下

这样我们可以不断进行n = n & (n - 1)直到n === 0 ， 说明没有一个1了。 这个时候我们消除了多少1变成一个1都没有了， 就说明n有多少个1了

# Review
> https://aaronparecki.com/2018/07/07/7/oauth-for-the-open-web



# Tips
> 

# Share
> 
