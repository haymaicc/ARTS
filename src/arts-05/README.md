# Algorithm
> https://leetcode.com/problems/maximum-depth-of-binary-tree/description/

```java
public int maxDepth(TreeNode root) {
     if(root == null){
        return 0;
    }
    if(root.left == null && root.right == null){
        return 1;
    }
    return 1+Math.max(maxDepth(root.left),maxDepth(root.right));
}
```

另一种解法：树的层次遍历

![树的层次遍历](https://github.com/azl397985856/leetcode/raw/master/assets/thinkings/binary-tree-traversal-bfs.gif)

具体做法：

1. 根节点入队列， 并入队列一个特殊的标识位，此处是 null

2. 出队列

3. 判断是不是 null， 如果是则代表本层已经结束。我们再次判断是否当前队列为空，如果不为空继续入队一个 null，否则说明遍历已经完成，我们什么都不不用做

4. 如果不为 null，说明这一层还没完，则将其左右子树依次入队列。

# Review
> https://www.recordedfuture.com/dark-web-reality/

# 暗网

* 暗网很小，与流行的冰山比喻是颠倒的
* 这些网站杂乱无章，不可靠
* 英语为主要语言
* 隐藏自身和神秘感，入站链接少

# Tips
> https://github.com/aalansehaiyang/technology-talk/blob/master/ops/linux-commands.md

## linux常用命令

### 一、CPU相关、进程
1. 查看 cpu 硬件配置
    
    ```java
    less /proc/cpuinfo 
    ```
    
2. top 命令
实时显示各种系统资源使用情况及进程状态

详细命令参数：
* h: 显示帮助
* c：显示详细的命令参数
* M：按照占用内存大小（%MEM 列）对进程排序；
* P：按照 CPU 使用率( %CPU 列）对进程排序；
* u：显示指定用户的进程。默认显示所有进程；
* T：根据累计运行时间排序

3. 统计一个进程下的线程数

    ```java
    cat /proc/${pid}/status
    ```
    
4. 查看所有进程

    ```java
    ps -ef
    ps -ef|grep java
    ```
5. ulimit -a （显示当前的各种用户进程限制）
    

### 二、内存相关
1. vmstat
Virtual Memory Statistics，统计进程、内存、io、cpu等的活动信息。对于多CPU系统，vmstat打印的是所有CPU的平均输出
2. sar -r
3. cat /proc/meminfo
4. free -m

### 三、IO及网络
1. tsar --traffic：显示网络带宽
2. netstat
一般用于检验本机各端口的网络连接情况。netstat是在内核中访问网络及相关信息的程序，它能提供TCP连接，TCP和UDP监听，进程内存管理的相关报告。
3. iostat
iostat是I/O statistics（输入/输出统计）的缩写，主要的功能是对系统的磁盘I/O操作进行监视。它的输出主要显示磁盘读写操作的统计信息，同时也会给出CPU使用情况。同vmstat一样，iostat也不能对某个进程进行深入分析，仅对系统的整体情况进行分析。
4. sar -b：磁盘状态历史记录

### 四、文件
1. lsof (一切皆文件) 
查看你进程开打的文件，打开文件的进程，进程打开的端口(TCP、UDP)
2. df
```shell
df -hl
磁盘的使用情况
```
3. du
```shell
du -hl
当前目录下的最叶子目录的大小
```
```shell
du -sch * 
当前目录下的各目录的大小
```
4. find
文件查找
```shell
find . -name sys.log  
当前目录下查找sys.log文件

find . -name "sy*log"
查找文件支持通配符

find . -size +20M
查找当前目录下大小超过20M的文件  

find . -size +20M | xargs ls -lh
查找当前目录下大小超过20M的文件，并计算文件大小

find -type f -printf '%s %p\n' |sort -nr | head  
查找占用空间最大的10个文件
```
5. tail
从指定点开始将文件标准输出
```shell
显示文件最后5行内容
tail -n 5 test.log

实时显示文件内容
tail -f test.log
```

### 五、用户
```shell
w                       查看活动用户
id <用户名>              查看指定用户信息
last                    查看用户登录日志
cut -d: -f1 /etc/passwd 查看系统所有用户
cut -d: -f1 /etc/group  查看系统所有组
crontab -l              查看当前用户的计划任务
```

# Share
>https://www.zhihu.com/question/19638322/answer/17899852

##  Scrum 的目的和限制
Scrum 的目的在于两点:
1. 适应变化。Scrum 的一个基本假设，就是外部需求模糊而难以理解。Scrum 对此的理念是：让客户直接看到半成品，他们才知道自己要什么。很多 Scrum 的原则都是围绕如何解决这个问题的：比如每个 Sprint 结束时由 Product Owner 为客户进行展示，又比如任务细化一般不超过一个 Sprint。理解了这一点，才会理解为什么 Scrum 似乎总在变化，因为需求总在变化。快速迭代。Scrum 的另一个基本假设，是团队生存在一个快速变化且充满竞争的世界。如果自己一年半才能发布一个新版本，而竞争对手半年就能发布，那么几年之内，我们就会被对手甩得远远的。Scrum 对此的理念是：发布即 Milestone（里程碑），宁可每次发布二十个功能发布五次，也不要在内部搞五个 Milestone 然后一口气发布一百个功能。理解了这一点，才会理解为什么 Scrum 会认为发布时砍功能是一种正常情况而非一种失败。
2. 快速迭代。Scrum 的另一个基本假设，是团队生存在一个快速变化且充满竞争的世界。如果自己一年半才能发布一个新版本，而竞争对手半年就能发布，那么几年之内，我们就会被对手甩得远远的。Scrum 对此的理念是：发布即 Milestone（里程碑），宁可每次发布二十个功能发布五次，也不要在内部搞五个 Milestone 然后一口气发布一百个功能。理解了这一点，才会理解为什么 Scrum 会认为发布时砍功能是一种正常情况而非一种失败。

Scrum 不能做什么:
1. 因为发布周期缩短，团队没有能力保证作出的每一个决定都正确，很多开销都必须花在试错上；
2. 快速发布实际上导致 Scrum 团队的抗风险能力弱于瀑布模型团队，因为一个人的离职或病假都可能对单一功能的进度造成影响，不利于短期频繁发布。

## others
* 50% percent of our decisions are wrong. Fail fast, learn fast. （我们作出的决定中， 50% 都是错误的。早早失败，早早学习。）
* No matter what you want to do, choose what is good for your team.（无论你选择做什么，选择对你的团队有利的事）

