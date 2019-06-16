# Algorithm
> https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0){
            return 0;
        }
        int profit = 0, minPrice = prices[0];
		for (int i = 0; i < prices.length; i++) {
			if (prices[i] > minPrice) {
				profit = Math.max(profit, prices[i] - minPrice);
			}
			else {
				minPrice = prices[i];
			}
		}
		return profit;
    }
}
```

# Review
> http://highscalability.com/blog/2019/4/8/from-bare-metal-to-kubernetes.html

## 怎么将裸机基础设施迁移到k8s集群

### 早期的基础架构
* 共享 VPS (性能极差)
* 独享 VPS (Rackspace 公司， 单点严重，价格贵)
* OVH 便宜的云计算服务，比 Rackspace 便宜，且性能更高

### 硬件架构
* 遇到了可维护性和可扩展性问题
* 扩展开发流程

### Docker
解决开发环境的协作问题，利用 docker 构建开发环境

### Kubernates
根据负载可拓展架构

k8s 学习：
Most importantly understanding containers, Pods, Deployments and Services and how they all fit together. Then in order, ConfigMaps, Secrets, Daemonsets, StatefulSets, Volumes, PersistentVolumes and PersistentVolumeClaims.

[Helm](https://github.com/helm/helm)帮助您管理Kubernetes应用程序 - Helm Charts可帮助您定义，安装和升级最复杂的Kubernetes应用程序。


# Tips
> https://github.com/aalansehaiyang/technology-talk/blob/master/open-source-framework/Goole-Guava.md

## Google Guava

---

* [Guava 源码](https://github.com/google/guava/wiki/CachesExplained#population)


---
### pom依赖

```
<dependency>
  	<groupId>com.google.guava</groupId>
  	<artifactId>guava</artifactId>
  	<version>13.0.1</version>
</dependency>

```

### 一、缓存相关
如果服务上要使用本地缓存，可以考虑使用guava框架。Guava Cache与ConcurrentMap很相似，但也不完全一样。最基本的区别是ConcurrentMap会一直保存所有添加的元素，直到显式地移除。相对地，Guava Cache为了限制内存占用，通常都设定为自动回收元素。在某些场景下，尽管LoadingCache 不回收元素，它也是很有用的，因为它会自动加载缓存。

#### Guava Cache适用于：

* 你愿意消耗一些内存空间来提升速度。
* 你预料到某些键会被查询一次以上。
* 缓存中存放的数据总量不会超出内存容量。

```
 private Cache<String, UserAuthTokenRecord> localCache = CacheBuilder.newBuilder().maximumSize(200000)
            .initialCapacity(100000).expireAfterWrite(30, TimeUnit.SECONDS).<String, UserAuthTokenRecord>build();
            
```

### 缓存回收

* ##### 基本容量的回收

  1）设置了初始值和最大容量上限，如果逼近容量上限，就会触发回收机制。
  
* ##### 定时回收

	1）expireAfterAccess(long, TimeUnit)：缓存项在给定时间内没有被读/写访问，则回收。请注意这种缓存的回收顺序和基于大小回收一样。
	
	2）expireAfterWrite(long, TimeUnit)：缓存项在给定时间内没有被写访问（创建或覆盖），则回收。如果认为缓存数据总是在固定时候后变得陈旧不可用，这种回收方式是可取的。

* ##### 基于引用的回收

### 二、常用的工具类

##### 1.创建一个集合
com.google.common.collect.Lists.newArrayList(E... elements)

##### 2.创建指定容量大小的集合

注：避免使用过程中，容量不足引发的扩容带来的性能损耗。

com.google.common.collect.Lists.newArrayListWithCapacity(int)

创建指定大小的的List；

com.google.common.collect.Maps.newHashMapWithExpectedSize(int)

创建指定大小的的Map；

##### 3.连接器[Joiner]

用分隔符把字符串序列连接起来也可能会遇上不必要的麻烦。如果字符串序列中含有null，那连接操作会更难。Fluent风格的Joiner让连接字符串更简单。

```
Joiner joiner = Joiner.on("; ").skipNulls();
return joiner.join("Harry", null, "Ron", "Hermione");
```

上述代码返回”Harry; Ron; Hermione”。另外，useForNull(String)方法可以给定某个字符串来替换null，而不像skipNulls()方法是直接忽略null。

Joiner也可以用来连接对象类型，在这种情况下，它会把对象的toString()值连接起来。

```
Joiner.on(",").join(Arrays.asList(1, 5, 7)); // returns "1,5,7"
```

警告：joiner实例总是不可变的。用来定义joiner目标语义的配置方法总会返回一个新的joiner实例。这使得joiner实例都是线程安全的，你可以将其定义为static final常量。

##### 4.拆分器[Splitter]

JDK内建的字符串拆分工具有一些古怪的特性。比如，String.split悄悄丢弃了尾部的分隔符。 问题：”,a,,b,”.split(“,”)返回？

“”, “a”, “”, “b”, “”

null, “a”, null, “b”, null

“a”, null, “b”

“a”, “b”


以上都不对，””, “a”, “”, “b”。只有尾部的空字符串被忽略了。 Splitter使用令人放心的、直白的流畅API模式对这些混乱的特性作了完全的掌控。

```
Splitter.on(',')
        .trimResults()
        .omitEmptyStrings()
        .split("foo,bar,,   qux");
```

上述代码返回Iterable<String>，其中包含”foo”、”bar”和”qux”。Splitter可以被设置为按照任何模式、字符、字符串或字符匹配器拆分。

**拆分器工厂**

|方法|	描述|	范例|
|------------- |------------- |------------- |
|Splitter.on(char)|	按单个字符拆分|	Splitter.on(‘;’)|
|Splitter.on(CharMatcher)	|按字符匹配器拆分	|Splitter.on(CharMatcher.BREAKING_WHITESPACE)|
|Splitter.on(String)|	按字符串拆分	|Splitter.on(“,   “)|
|Splitter.on(Pattern)Splitter.onPattern(String)|	按正则表达式拆分	|Splitter.onPattern(“\r?\n”)|
|Splitter.fixedLength(int)	|按固定长度拆分；最后一段可能比给定长度短，但不会为空。	|Splitter.fixedLength(3)|

**拆分器修饰符**

|方法|	描述|
|-------------|-------------|
|omitEmptyStrings()	|从结果中自动忽略空字符串|
|trimResults()	|移除结果字符串的前导空白和尾部空白|
|trimResults(CharMatcher)|	给定匹配器，移除结果字符串的前导匹配字符和尾部匹配字符|
|limit(int)|	限制拆分出的字符串数量|

如果你想要拆分器返回List，只要使用Lists.newArrayList(splitter.split(string))或类似方法。 警告：splitter实例总是不可变的。用来定义splitter目标语义的配置方法总会返回一个新的splitter实例。这使得splitter实例都是线程安全的，你可以将其定义为static final常量。


# Share
> https://github.com/aalansehaiyang/technology-talk/blob/master/system-architecture/%E6%9E%B6%E6%9E%84%E6%80%9D%E6%83%B3.md

## 架构思想

---

关于什么是架构，一种比较通俗的说法是 “最高层次的规划，难以改变的决定”，这些规划和决定奠定了事物未来发展的方向和最终的蓝图。

从这个意义上说，人生规划也是一种架构。选什么学校、学什么专业、进什么公司、找什么对象，过什么样的生活，都是自己人生的架构。

具体到软件架构，维基百科是这样定义的：“有关软件整体结构与组件的抽象描述，用于指导大型软件系统各个方面的设计”。系统的各个重要组成部分及其关系构成了系统的架构，这些组成部分可以是具体的功能模块，也可以是非功能的设计与决策，他们相互关系组成一个整体，共同构成了软件系统的架构。


架构其实就是把复杂的问题抽象化、简单化，可能你会觉得“说起来容易但做起来难”，如何能快速上手。可以多观察，根据物质决定意识，借助生活真实场景（用户故事，要很多故事）来还原这一系列问题，抓住并提取核心特征。


#### 架构思想

CPU运算速度>>>>>内存的读写速度>>>>磁盘读写速度


* 满足业务发展需求是最高准则
* 业务建模，抽象和枚举是两种方式，需要平衡，不能走极端
* 模型要能更真实的反应事物的本质，不是名词概念的堆砌，不能过度设计
* 基础架构最关键的是分离不同业务领域、不同技术领域，让整个系统具有持续优化的能力。
* 分离基础服务、业务规则、业务流程，选择合适的工具外化业务规则和业务流程
* 分离业务组件和技术组件，高类聚，低耦合 - 业务信息的执行可以分散，但业务信息的管理要尽量集中
* 不要让软件的逻辑架构与最后物理部署绑死 - 选择合适的技术而不是高深的技术，随着业务的发展调整使用的技术
*  好的系统架构需要合适的组织架构去保障 - 团队成员思想的转变，漫长而艰难
*  业务架构、系统架构、数据模型


#### 面对一块新业务，如何系统架构？
* 业务分析：输出业务架构图，这个系统里有多少个业务模块，从前台用户到底层一共有多少层。
* 系统划分：根据业务架构图输出系统架构图，需要思考的是这块业务划分成多少个系统，可能一个系统能支持多个业务。基于什么原则将一个系统拆分成多个系统？又基于什么原则将两个系统合并成一个系统？
* 系统分层：系统是几层架构，基于什么原则将一个系统进行分层，分成多少层？
* 模块化：系统里有多少个模块，哪些需要模块化？基于什么原则将一类代码变成一个模块。


#### 如何模块化

* 基于水平切分。把一个系统按照业务类型进行水平切分成多个模块，比如权限管理模块，用户管理模块，各种业务模块等。
* 基于垂直切分。把一个系统按照系统层次进行垂直切分成多个模块，如DAO层，SERVICE层，业务逻辑层。
* 基于单一职责。将代码按照职责抽象出来形成一个一个的模块。将系统中同一职责的代码放在一个模块里。比如我们开发的系统要对接多个渠道的数据，每个渠道的对接方式和数据解析方式不一样，为避免不同渠道代码的相互影响，我们把各个渠道的代码放在各自的模块里。
* 基于易变和不易变。将不易变的代码抽象到一个模块里，比如系统的比较通用的功能。将易变的代码放在另外一个或多个模块里，比如业务逻辑。因为易变的代码经常修改，会很不稳定，分开之后易变代码在修改时候，不会将BUG传染给不变的代码。

#### 提升系统的稳定性

* 流控

双11期间，对于一些重要的接口（比如帐号的查询接口，店铺首页）做流量控制，超过阈值直接返回失败。
另外对于一些不重要的业务也可以考虑采用降级方案，大促—>邮件系统。根据28原则，提前将大卖家约1W左右在缓存中预热，并设置起止时间，活动期间内这部分大卖家不发交易邮件提醒，以减轻SA邮件服务器的压力。

* 容灾

最大程度保证主链路的可用性，比如我负责交易的下单，而下单过程中有优惠的业务逻辑，此时需要考虑UMP系统挂掉，不会影响用户下单（后面可以通过修改价格弥补），采用的方式是，如果优惠挂掉，重新渲染页面，并增加ump屏蔽标记，下单时会自动屏蔽ump的代码逻辑。
另外还会记录ump系统不可用次数，一定时间内超过阈值，系统会自动报警。

* 稳定性

第三方系统可能会不稳定，存在接口超时或宕机，为了增加系统的健壮性，调用接口时设置超时时间以及异常捕获处理。

* 容量规划

做好容量规划、系统间强弱依赖关系梳理。
如：冷热数据不同处理，早期的订单采用oracle存储，随着订单的数量越来越多，查询缓慢，考虑数据迁移，引入历史表，将已归档的记录迁移到历史表中。当然最好的方法是分库分表。


#### 分布式架构

* 分布式系统
* 分布式缓存
* 分布式数据


#### API 和乐高积木有什么相似之处？

相信我们大多数人在儿童时期都喜欢玩乐高积木。乐高积木的真正乐趣和吸引力在于，尽管包装盒外面都带有示意图片，但你最终都可以随心所欲得搭出各种样子或造型。

对 API 的最佳解释就是它们像乐高积木一样。我们可以用创造性的方式来组合它们，而不用在意它们原本的设计和实现意图。

你可以发现很多 API 和乐高积木的相似之处：

- 标准化：通用、标准化的组件，作为基本的构建块（building blocks）；

- 可用性：强调可用性，附有文档或使用说明；

- 可定制：为不同功能使用不同的API；

- 创造性：能够组合不同的 API 来创造混搭的结果；

乐高和 API 都有超简单的界面/接口，并且借助这样简单的界面/接口，它可以非常直观、容易、快速得构建。

虽然乐高和 API 一样可能附带示意图片或使用文档，大概描述了推荐玩法或用途，但真正令人兴奋的结果或收获恰恰是通过创造力产生的。

让我们仔细地思考下上述的提法。在很多情况下，API 的使用者构建出了 API 的构建者超出预期的服务或产品，API 使用者想要的，和 API 构建者认为使用者想要的，这二者之间通常有个断层。事实也确实如此，在 IoT 领域，我们使用 API 创造出了一些非常有创造性的使用场景。
