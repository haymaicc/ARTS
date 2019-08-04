# Algorithm
> https://github.com/azl397985856/leetcode/blob/master/problems/190.reverse-bits.md

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int ret = 0;
		for (int i = 0; i < 32; i++) {
			ret = (ret << 1) + (n & 1);
			n = n >> 1;
		}
        return ret;
    }
}
```

* n从高位开始逐步左， res从低位（0）开始逐步右移
* 逐步判断，如果该位是1，就res + 1 , 如果是该位是0， 就res + 0
* 32位全部遍历完，则遍历结束

# Review
> https://boredhacking.com/tor-webscraping-proxy/


# Tips
> https://juejin.im/entry/58fada5d570c350058d3aaad

# Share
> https://juejin.im/post/5b65122be51d4517c564d54f
