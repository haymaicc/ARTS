# Algorithm
> https://leetcode.com/problems/min-stack/description/

```java
class MinStack {
    int min = Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack<>();
    public void push(int x) {
        if(x <= min){          
            stack.push(min);
            min=x;
        }
        stack.push(x);
    }

    public void pop() {
        // if pop operation could result in the changing of the current minimum value, 
        // pop twice and change the current minimum value to the last minimum value.
        if(stack.pop() == min) min=stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return min;
    }
}
```

# Review
> https://eng.fromatob.com/post/2019/05/why-were-switching-to-grpc/

## gRPC

### 清晰的接口格式
.proto 文件定义接口格式（其实也没有比rest更清晰吧？）

### 对流的支持
不需要轮训，这是一个优势


# Tips
> https://github.com/aalansehaiyang/technology-talk/blob/master/open-source-framework/commons-lang3.md

## commons-lang3

常用工具类：
内容虽然有点多，但我们使用最多还是一些有用的包含static方法的Util类。
* StringUtils – 处理String的核心类，提供了相当多的功能；
* NumberUtils - 类型转换（String->Long）；取最大最小值；比较大小。所有操作都不会抛出异常，如果转换不成功返回0,0.0d,0.0f等形式，转换操作也可以指定默认值。
* DateUtils -日期相关；是否同一天；时间+x；字符串转换成Date
* ArrayUtils – 用于对数组的操作，如添加、查找、删除、子数组、倒序、元素类型转换等；
* SystemUtils – 在java.lang.System基础上提供更方便的访问，如用户路径、Java版本、时区、操作系统等判断；
* WordUtils – 用于处理单词大小写、换行等。
* StringEscapeUtils – 用于正确处理转义字符，产生正确的Java、JavaScript、HTML、XML和SQL代码；
* CharRange – 用于设定字符范围并做相应检查；
* ClassUtils – 用于对Java类的操作，不使用反射；
* Validate – 提供验证的操作，有点类似assert断言；


## commons-io
封装了一些常用工具类方法，用于IO的各种操作:
* Utility class ，提供一些静态方法来满足一些常用的业务场景
* Input ， InputStream 和 Reader 实现
* Output ， OutputStream 和 Writer 实现
* Filters ， 多种文件过滤器实现（定义了 IOFileFilter接口，同时继承了 FileFilter 和 FilenameFilter 接口）
* comparator包， 文件比较，提供了多种 java.util.Comparator 实现

### 常用工具类
* IOUtils
    
    提供各种静态方法，用于处理读，写和、拷贝，这些方法基于InputStream、OutputStream、Reader 和 Writer
    
    ```java
    InputStream in = new URL( "http://commons.apache.org" ).openStream();
    try { System.out.println( IOUtils.toString( in ) ); } finally { IOUtils.closeQuietly(in); }
    ```
* FileUtils
    
    提供各种静态方法，基于File对象工作，包括读、写、拷贝、比较文件
    ```java
      File file = new File("/commons/io/project.properties"); 
      List lines = FileUtils.readLines(file, "UTF-8");
    ```
    
* LineIterator

    提供灵活的方式操作基于行的文件。通过FileUtils 或 IOUtils中的静态方法，可以直接创建一个实例

    ```java
      LineIterator it = FileUtils.lineIterator(file, "UTF-8");
      try { 
        while (it.hasNext()) { 
          String line = it.nextLine(); /// do something with line 
         } 
      } finally { LineIterator.closeQuietly(it); }
    ```


# Share
> https://juejin.im/post/5d0b1381e51d455a694f9544

## WebSockets 与长轮询的较量

### 长轮询的利与弊
优点
* 长轮询是在 XMLHttpRequest 之后实现的，它几乎得到了设备的普遍支持，因此通常很少需要有进一步的备选方案。但是，在必须处理异常的情况下，或者在服务器可查询新数据但不支持长轮询（更不用说其他更现代的技术标准）的情况下，基本轮询有时仍然有些用处，并且可以使用 XMLHttpRequest 或通过 JSONP 利用简单的 HTML 脚本标签。
缺点
* 长轮询大量占据服务器资源。
* 可靠的消息排序 可能是长轮询的一个问题，因为来自同一个客户端的多个 HTTP 请求可能同时运行。举个例子，如果一个客户端打开两个浏览器选项卡，使用相同的服务器资源，并且客户端应用程序正在将数据持久化到本地存储区（如 localStorage 或 IndexedDb），则无法保证重复数据不会被多次写入。
* 根据服务端实现的不同，一个客户端对消息的确认接收也可能导致另一个客户端根本不会收到预期的消息，因为服务端可能错误地认为客户端已经收到了它所期望的数据。

### WebSockets 的利与弊
优点
* WebSockets 保持一个唯一的连接打开，同时消除长轮询的延迟问题。
* WebSockets 通常不使用 XMLHttpRequest，因此，当我们每次需要从服务器获取更多的信息时，无需发送头部数据。反过来说，这又减少了数据发送到服务器时需要付出的高昂的数据负载代价。

缺点
* 当连接终止时，WebSockets 无法自动恢复连接 —— 这是需要你自己实现的部分，也是导致存在许多 客户端库 的原因。
* 早于 2011 年的浏览器无法支持 WebSocket 连接 —— 但这一点越来越无关紧要。

### 为什么 WebSocket 协议是更好的选择？
长轮询在服务器上占用更多的资源，而 WebSockets 在服务器上占用的空间很少。长轮询还需要在服务器与许多设备之间进行多次通信。而不同的网关对于一个常规连接允许保持打开的时间有不同的标准。如果连接打开时间太久，其进程可能会被杀死，甚至当这个进程正在处理一些重要的事情时。

使用 WebSockets 构建应用的理由：
* 全双工异步消息传送。换句话说，客户端和服务器都可以独立地相互传输消息。
* WebSockets 无需任何配置即可通过大多数防火墙。
* 良好的安全模式（基于原始的安全模式）。
