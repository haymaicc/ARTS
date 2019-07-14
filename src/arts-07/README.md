# Algorithm
> https://leetcode.com/problems/single-number/description/

```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int i : nums) {
            result = i ^ result;
        }
        return result;
    }
}
```

* 异或的性质 两个数字异或的结果a^b是将 a 和 b 的二进制每一位进行运算，得出的数字。 运算的逻辑是 如果同一位的数字相同则为 0，不同则为 1
* 异或的规律
* 任何数和本身异或则为0
* 任何数和 0 异或是本身
* 很多人只是记得异或的性质和规律，但是缺乏对其本质的理解，导致很难想到这种解法
* bit 运算

# Review
> https://datawhatnow.com/things-you-are-probably-not-using-in-python-3-but-should/

## Python 3 新特性

### f-strings (3.6+)
```python
user = "Jane Doe"
action = "buy"
log_message = 'User {} has logged in and did an action {}.'.format(
  user,
  action
)
print(log_message)
```
Python 3提供了一种通过f-string进行字符串插值的灵活方法。
```python
user = "Jane Doe"
action = "buy"
log_message = f'User {user} has logged in and did an action {action}.'
print(log_message)
# User Jane Doe has logged in and did an action buy.
```

### Pathlib (3.4+)
```python
from pathlib import Path

root = Path('post_sub_folder')
print(root)
# post_sub_folder

path = root / 'happy_user'

# Make the path absolute
print(path.resolve())
# /home/weenkus/Workspace/Projects/DataWhatNow-Codes/how_your_python3_should_look_like/post_sub_folder/happy_user
```

### Type hinting (3.5+)
```python
def sentence_has_animal(sentence: str) -> bool:
  return "animal" in sentence
sentence_has_animal("Donald had a farm without animals")
# True
```

### Enumerations (3.4+)
```python
from enum import Enum, auto
class Monster(Enum):
    ZOMBIE = auto()
    WARRIOR = auto()
    BEAR = auto()
    
print(Monster.ZOMBIE)
# Monster.ZOMBIE
```

### Built-in LRU cache (3.2+)
```python
from functools import lru_cache
@lru_cache(maxsize=512)
def fib_memoization(number: int) -> int:
    if number == 0: return 0
    if number == 1: return 1
    
    return fib_memoization(number-1) + fib_memoization(number-2)
start = time.time()
fib_memoization(40)
print(f'Duration: {time.time() - start}s')
# Duration: 6.866455078125e-05s
```

### Extended iterable unpacking (3.0+)
```python
head, *body, tail = range(5)
print(head, body, tail)
# 0 [1, 2, 3] 4
py, filename, *cmds = "python3.7 script.py -n 5 -l 15".split()
print(py)
print(filename)
print(cmds)
# python3.7
# script.py
# ['-n', '5', '-l', '15']
first, _, third, *_ = range(10)
print(first, third)
# 0 2
```

### Data classes (3.7+)
原来构造方法这样写：
```python
class Armor:
    
    def __init__(self, armor: float, description: str, level: int = 1):
        self.armor = armor
        self.level = level
        self.description = description
                 
    def power(self) -> float:
        return self.armor * self.level
    
armor = Armor(5.2, "Common armor.", 2)
armor.power()
# 10.4
print(armor)
# <__main__.Armor object at 0x7fc4800e2cf8>
```
python 3.7 之后可以用 dataclass 注解
```python
from dataclasses import dataclass
@dataclass
class Armor:
    armor: float
    description: str
    level: int = 1
    
    def power(self) -> float:
        return self.armor * self.level
    
armor = Armor(5.2, "Common armor.", 2)
armor.power()
# 10.4
print(armor)
# Armor(armor=5.2, description='Common armor.', level=2)
```

### Implicit namespace packages (3.3+)
构造 python 的包一般通过一个文件夹，里面有一个 __init__.py 文件，例如：
```
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```
可以用隐式的包命名空间
```
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

# Tips
> https://mp.weixin.qq.com/s/WkBavHJyfCQCo_P1Vmm03A

idea 使用

## 懒人必备快捷键

* A. 按【鼠标中键】快速打开智能提示，取代alt+enter 。

    File->Settings-> Keymap-> 搜索 Show Intention Actions -> 添加快捷键为鼠标中键。

* B. 按【F2】快速修改文件名，告别双手操作。

    File->Settings-> Keymap-> 搜索 Rename -> 将快捷键设置为F2 。

* C. 按【F3】直接打开文件所在目录，浏览一步到位。

    File->Settings-> Keymap-> 搜索 Show In Explorer -> 将快捷键设置为F3 。

* D. 按【Ctrl+鼠标右键】直接打开实现类，方便开发查询。

    File->Settings-> Keymap-> 搜索 implementation->  Add Mouse Shortcut 将快捷键设置为Ctrl+ 鼠标右键。

## 重度强迫症患者

* A.取消大小写敏感，让自动提示更齐全！  

    File | Settings | Editor | General | Code Completion Case | Sensitive Completion = None。

* B.隐藏开发工具的配置目录 例如 *.idea;*.iml 

    File | Settings | File Types | 在末尾加上 *.idea;*.iml

* C.收起注释，让源码阅读更为清爽！ 

    File -> Settings -> Editor -> General -> Code Folding ->  Documentation comments 勾选。

    如何想快速一键打开全部注释，则单击鼠标右键，选择Folding -> Expand Doc comments 。

## 插件
> https://mp.weixin.qq.com/s/b994EvxMRZIvPedHi8x2Yw
* GsonFormat
    地址：https://plugins.jetbrains.com/plugin/7654-gsonformat
    一键根据json文本生成java类  非常方便
* Maven Helper
    地址：https://plugins.jetbrains.com/plugin/7179-maven-helper
    一键查看maven依赖，查看冲突的依赖，一键进行exclude依赖
    对于大型项目 非常方便
* GenerateAllSetter
    地址：https://plugins.jetbrains.com/plugin/9360-generateallsetter
    一键调用一个对象的所有set方法并且赋予默认值 在对象字段多的时候非常方便
* VisualVM Launcher
    地址：https://plugins.jetbrains.com/plugin/7115-visualvm-launcher
    运行java程序的时候启动visualvm，方便查看jvm的情况 比如堆内存大小的分配
    某个对象占用了多大的内存，jvm调优必备工具


# Share
> https://github.com/aalansehaiyang/technology-talk/blob/master/system-architecture/%E7%BC%96%E7%A0%81%E5%89%8D3000%E9%97%AE.md

## 编码前3000问

---

> 当我们接到产品的需求的时候，别着急马上投入开发，先要了解需求的来龙去脉，多思考，尽可能将里面潜在的坑挖出来，这样上线后才会更好的满足需求，代码的扩展性、代码生命周期才会更长。

---

### 下面简单列了一些应该思考的问题：


*	多去了解业务功能背后的想法，也许产品只是口渴了而不是真的想喝牛奶，技术同学可以从技术角度给产品同学一些启发，最终大家达成新的业务共识。

	> 一名优秀的工程师至少可以抵半个产品经理！

*	新功能会不会对历史业务产生影响？如果会的话，要把影响的范围尽可能全的罗列出来，产品和技术共同探讨老业务的兼容问题

*	如果涉及底层老的数据库表重构，要考虑新、老数据如何平稳迁移，不影响线上用户的正常访问

*	是否存在并发，如何保证数据质量？要采用什么锁机制？

*	做好概要设计方案、详情设计方案，并组织所有相关人员参加评审，如果涉及数据库变动，最好叫上DBA。做方案尽量多想一想，如果担心老业务吃不透，可以叫上一些老员工，先整体再细节再整体，做到有点有面，一定要以全局性视野对待项目，否则很容易陷入误区。

*	容量规划。新功能上线后，数据量有多大？后续每日预估新增多少数据？采用什么形式的存储？是关系型数据库（如mysql）? 要不要分库分表？还是采用Nosql存储？

*	数据是否存在冷数据、热数据之分（例如微博），是否要分开存储？

*	尽可能采用服务化形式，但是抽象到何种程度，要视具体的业务而定，尽量朝着高内聚、低耦合设计原则。

    * 集中式调用改为HSF分布式远程调用，走的网络调用。为了提高性能，封装client二方包，通过xml bean配置控制逻辑，先从tair中查询，然后再走远程调用。
    * 如果有多处地方代码用到同样的模型数据，最好能过上下文的方式传递，避免每次用到都走接口查询

*	模块化、组件化，具备乐高积木的特性

*   多使用一些设计模式，提升系统的可扩展性

*	尽量往平台方向思考，但要注意控制成本，即便一期做不到大而全，但一定要留好扩展，便于后续的不断迭代。

*	如果是新应用，另起炉灶，要做好技术选型，最好选一些主流技术框架，切勿凭个人喜好，最后搞成百花齐放，后面的维护成本极高

	> 统一、标准化是一个亘古不变的话题，这一点非常重要

*	如果是跨部门跨团队合作，需要提前约定好交互方式，联调时间，并要想一想，如果一方挂了，另一方如何做才能将影响降到最小。

*	对于很多技术改造，可能会配置开关，做好开关两面的测试工作，必要时可以紧急切换开关降低影响

*	接口响应时间。是否需要引入缓存，缓存的数据如何维护？数据预热、数据有效期，空间不足、缓存的命中率怎么样？

*	接口的最大并发量，需要做性能压测，了解系统能支撑的上限，便于大促活动时机器扩容

*	接口的容错性，如果出现意外情况时，尽量保住核心业务，不受边缘业务或非核心接口的影响

*	有条件的话，做好接口的流量控制，配置阈值，超过预设值能自我保护，并有对应的业务提示。

*	做好单元测试、项目代码 code review

*	发布时，要提前准备发布计划，以及回滚计划