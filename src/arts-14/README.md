# Algorithm
> https://leetcode.com/problems/house-robber/description/

```java
class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[nums.length + 2];
		dp[0] = 0;
		dp[1] = 0;

		for (int i = 2; i < nums.length + 2; i++) {
			dp[i] = Math.max(dp[i - 2] + nums[i - 2], dp[i - 1]);
		}
        return dp[nums.length + 1];
    }
}
```

# Review
> http://rickcarlino.com/2019/07/20/what-were-cgi-scripts-html.html

## 历史技术 - CGI 脚本
CGI是早期Web的一种技术，它使能够构建简单（但有用）的动态Web应用程序

局限性：
* 每个请求一个进程，I/O 效率低
* 无法处理大规模应用程序的复杂性
* 由于服务器直接执行脚本，安全问题可能很容易蔓延（脚本共享HTTP服务器的权限）


# Tips

## Java ForkJoin 
https://github.com/haymaicc/ForkJoin/tree/master/src/main/java/mapreduce

# Share
> https://time.geekbang.org/column/article/76567

程序员总喜欢用技术去解决一切问题，但很多令人寝食难安的问题其实根本不是问题。之所以找不出更简单的解决方案，很多时候原因在于程序员被自己的思考局限住了。

想把工作做好，就需要不断扩大自己工作的上下文，多了解一下别人的工作逻辑是什么样的，认识软件开发的全生命周期。

扩大自己的上下文，除了能对自己当前的工作效率提高有帮助，对自己的职业生涯也是有好处的。随着你看到的世界越来越宽广，得到的机会也就越来越多。

Efficiency是盯住一点，提高效率，而Effective会站在更大的上下文去思考如何能以最优的方式得到想要的结果

手里拿着锤子，看什么都是钉子。
这样做是因为自己掌握的工具（包括思维）不够。能摆脱自身环境的束缚，从更高的维度解决问题是一种向上能力。我一直相信这样的观点：
想要到达什么位置，就先像坐在个位置的人那样做事、思考。