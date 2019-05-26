# Algorithm
> https://leetcode.com/problems/remove-duplicates-from-sorted-array/description

```java
public int removeDuplicates(int[] nums) {
    int size = nums.length;
    int cur = 0;
    for (int i = 0; i < size; i++) {
        if (nums[cur] != nums[i]) {
            cur++;
            nums[cur] = nums[i];
        }
    }
    return cur + 1;
}
```

# Review
> https://www.cloudflare.com/learning/serverless/glossary/serverless-vs-paas

## Serverless 和 PaaS的区别
![difference](https://www.cloudflare.com/img/learning/serverless/glossary/serverless-vs-paas/paas-serverless-comparison.svg)

* 扩展性
    * Serverless 可以立即，自动，按需扩展。PaaS需要有提前的预估。
* 定价
    * Serverless 计费精准
    * PaaS 按量计费但是不像 Serverless 那么精准或者按月固定费支付，计算能力也是事先决定的。
* 启动时间
    * Serverless 几乎立即激活
    * PaaS 需要更长的启动时间
* 提供的工具
    * PaaS 提供测试和调试的工具
    * Serverless 不能提供完整的构建和测试环境
* 边缘计算
    * Serverless 可以事先边缘计算
    * PaaS 底层还是依赖 IaaS，本质上还是仅在指定的机器运行，不支持边缘计算。

# Tips

## java服务器load飚高排查思路
Load 是指对计算机干活多少的度量（WikiPedia：the system load is a measure of the amount of work that a computer system is doing），简单的说是进程队列的长度。Load Average 就是一段时间 (1 分钟、5分钟、15分钟) 内平均 Load 

**通过uptime命令可以查看当前的load，如果值很高。一般情况是java某些线程长期占用资源、死锁、死循环等导致某个进程占用的CPU资源过高。大致可以从以下几个角度来排查：**
<br><br>

1.首先通过jps命令，查看当前进程id，如id为 28174

2.查看该进程下的线程资源使用情况

```
top -p 28174 –H  
```

```
32694 root      20   0 3249m 2.0g  11m S    2  6.4   3:31.12 java                      
28175 root      20   0 3249m 2.0g  11m S    0  6.4   0:00.06 java                   
28176 root      20   0 3249m 2.0g  11m S    0  6.4   1:40.79 java                   
28177 root      20   0 3249m 2.0g  11m S    0  6.4   1:41.12 java                   
28178 root      20   0 3249m 2.0g  11m S    0  6.4   1:41.11 java                   
28179 root      20   0 3249m 2.0g  11m S    0  6.4   1:41.33 java                   
28180 root      20   0 3249m 2.0g  11m S    0  6.4   1:41.58 java                   
28181 root      20   0 3249m 2.0g  11m S    0  6.4   1:40.36 java                   
28182 root      20   0 3249m 2.0g  11m S    0  6.4   1:41.02 java                   
28183 root      20   0 3249m 2.0g  11m S    0  6.4   1:40.96 java                   
28184 root      20   0 3249m 2.0g  11m S    0  6.4   4:38.30 java                   
28185 root      20   0 3249m 2.0g  11m S    0  6.4   0:00.46 java                   
28186 root      20   0 3249m 2.0g  11m S    0  6.4   0:01.83 java                   
28187 root      20   0 3249m 2.0g  11m S    0  6.4   0:00.00 java                   
28189 root      20   0 3249m 2.0g  11m S    0  6.4   0:00.01 java                   
28190 root      20   0 3249m 2.0g  11m S    0  6.4   0:49.01 java      

```

3.打印JAVA进程28174的堆栈信息

```
jstack 28174 >> stack.log 
```


4.将cpu消耗高的线程的pid换算为16进制

```
printf 0x%x 32694
```

转换后的16进制为 0x7fb6

5.从刚才的栈日志中查找该线程正在运行的方法

```
grep 0x7fb6  stack.log -a3
```   

```
"MSXMLProcessorThread" prio=10 tid=0x00002b469923a800 [color=darkred]nid=0x7fb6[/color] sleeping[0x00002b46b0200000]    
   java.lang.Thread.State: TIMED_WAITING (sleeping)    
        at java.lang.Thread.sleep(Native Method)    
        at com.adventnet.management.xml.MSXmlProcessor.listen(MSXmlProcessor.java:279)    
        at com.adventnet.management.xml.MSXmlProcessor.run(MSXmlProcessor.java:264)    
        at java.lang.Thread.run(Thread.java:619)    
```

6.另外也可以查找正在运行的线程，及线程处于运行状态的位置，从这些线程中来查找资源消耗过高的代码。

```
grep RUNNABLE stack.log  -a1   
```
![image](img/1364394978_1570.jpg)

```
less a.log |awk '{FS="java.lang.Thread.State:";print $2}'|sort |uniq -c |sort -nr  
```
![image](img/20130812130234781.jpeg)


7、查看当前jvm内存各堆区的占比

jmap -heap 8002

```
Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 786432000 (750.0MB)
   MaxNewSize               = 786432000 (750.0MB)
   OldSize                  = 1361051648 (1298.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 39845888 (38.0MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 398458880 (380.0MB)
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
New Generation (Eden + 1 Survivor Space):
   capacity = 707788800 (675.0MB)
   used     = 383204272 (365.4520721435547MB)
   free     = 324584528 (309.5479278564453MB)
   54.14104772497107% used
Eden Space:
   capacity = 629145600 (600.0MB)
   used     = 334989624 (319.4710006713867MB)
   free     = 294155976 (280.5289993286133MB)
   53.24516677856445% used
From Space:
   capacity = 78643200 (75.0MB)
   used     = 48214648 (45.98107147216797MB)
   free     = 30428552 (29.01892852783203MB)
   61.30809529622396% used
To Space:
   capacity = 78643200 (75.0MB)
   used     = 0 (0.0MB)
   free     = 78643200 (75.0MB)
   0.0% used
concurrent mark-sweep generation:
   capacity = 1361051648 (1298.0MB)
   used     = 985748664 (940.0831832885742MB)
   free     = 375302984 (357.9168167114258MB)
   72.4255148912615% used

49647 interned Strings occupying 5512480 bytes.
```


# Share

## SOLID
模块级编程的程序员构建软件的主要目标：
* 使软件可容忍被改动
* 使软件更容易被理解
* 构建可在多个软件系统中复用的组件

SOLID的主要内容：
* SRP：单一职责原则
    * 基于康威定律的一个推论：一个软件系统的最佳结构高度依赖于开发这个系统组织的内部结构。
    这样，每个系统模块都有且只有一个需要被修改的理由
* OCP：开闭原则
    * 如果软件系统想要更容易被改变，那么其设计就必须允许新增代码来修改系统行为，而非只能靠修改原来的代码
* LSP：里氏替换原则
    * 如果想用可替换组件来构建软件系统，那么这些组件就必须遵守同一个约定，以便让这些组件可以互相替换。
* ISP：接口隔离原则
    * 避免不必要的依赖
* DIP：依赖反转原则
    * 高层策略性代码不应该依赖实现底层细节的代码，实现底层细节的代码应该依赖高层策略性代码。
 
### 单一职责原则 (The Single Responsibility Principle)
SOLID 的第一个原则。这个原则表明模块或者类只应该做一件事。实际上，这意味着对程序功能的单个小更改，应该只需要更改一个组件。例如，更改密码验证复杂性的方式应该只需要更改程序的一部分。

理论上讲，这使代码更健壮，更容易更改。知道正在更改的组件只有一个功能，这意味着测试更改更容易。使用前面的例子，更改密码复杂性组件应该只影响与密码复杂性相关的功能。变更具有许多功能的组件可能要困难得多。

### 开闭原则 (The Open/Closed Principle)
SOLID 的第二个原则。这个原则指出实体（可以是类、模块、函数等）应该能够使它们的行为易于扩展，但是它们的扩展行为不应该被修改。

举一个假设的例子，想象一个能够将 Markdown 转换为 HTML 的模块。如果可以扩展模块，而不修改内部模块来处理新的 markdown 特征，而无需修改内部模块，则可以认为是开放扩展。如果用户不能修改处理现有 Markdown 特征的模块，那么它被认为是关闭修改。

这个原则与面向对象编程紧密相关，让我们可以设计对象以便于扩展，但是可以避免以意想不到的方式改变其现有对象的行为。

### 里氏替换原则 (The Liskov Substitution Principle)
SOLID 的第三个原则。该原则指出，如果组件依赖于类型，那么它应该能够使用该类型的子类型，而不会导致系统失败或者必须知道该子类型的详细信息。

举个例子，假设我们有一个方法，读取 XML 文档。如果该方法使用基类型 file，则从 file 派生的任何内容，都能用在该方法中。 如果 file 支持反向查找，并且 xml 解析器使用该函数，但是派生类型 network file 尝试反向查找时失败，则 network file 将违反该原则。

该原则与面向对象编程紧密相关，必须仔细建模、层次结构，以避免混淆系统用户。

### 接口隔离原则 (The Interface Segregation Principle)
SOLID 的第四个原则。该原则指出组件的消费者不应该依赖于它实际上不使用的组件函数。

举一个例子，假设我们有一个方法，读取 XML 文档。它只需要读取文件中的字节，向前移动或向后移动。如果由于文件结构不相关（例如更新文件安全性的权限模型），需要更新此方法，则该原则已失效。文件最好实现 可查询流 接口，并让 XML 读取器使用该接口。

该原则与面向对象编程紧密相关，其中接口，层次结构和抽象类型用于不同组件的 minimise the coupling。 Duck typing 是一种通过消除显式接口来强制执行该原则的方法。

### 依赖反转原则 (The Dependency Inversion Principle)
SOLID 的第五个原则。该原则指出，更高级别的协调组件不应该知道其依赖项的详细信息。

举个例子，假设我们有一个从网站读取元数据的程序。我们假设主要组件必须知道下载网页内容的组件，以及可以读取元数据的组件。如果我们考虑依赖反转，主要组件将仅依赖于可以获取字节数据的抽象组件，然后是一个能够从字节流中读取元数据的抽象组件，主要组件不需要了解 TCP、IP、HTTP、HTML 等。

这个原则很复杂，因为它似乎可以反转系统的预期依赖性（因此得名）。实践中，这也意味着，单独的编排组件必须确保抽象类型的正确实现被使用（例如在前面的例子中，必须提供元数据读取器组件、HTTP 文件下载功能和 HTML 元标签读取器）。然后，这涉及诸如 Inversion of Control 和 Dependency Injection 之类的模式。