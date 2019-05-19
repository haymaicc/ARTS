# Algorithm
> https://leetcode.com/problems/valid-parentheses/description

```java
class Solution {
    public boolean isValid(String s) {
        Stack stack = new Stack<Character>();
	Map<Character, Character> mappers = new HashMap<>(3);
	mappers.put('{', '}');
	mappers.put('[', ']');
	mappers.put('(', ')');
	for (int i = 0; i < s.length(); i++) {
		char c = s.charAt(i);
		if ("{[(".indexOf(c) != -1) {
			stack.push(c);
		} else {
	if(stack.isEmpty()){
	    return false;
	}
	char pop =  mappers.get(stack.pop());
	if(pop != c){
	    return false;
	}
		}
	}
	return stack.isEmpty();
    }
}
```

# Review
> https://blog.kowalczyk.info/article/19f2fe97f06a47c3b1f118fd06851fad/lessons-learned-porting-50k-loc-from-java-to-go.html

怎么把5W行java代码移植到go
* 依赖大量的自动化测试和跟踪代码覆盖
    * AppVeyor 测试
    * [Codecov.io](https://codecov.io/) 代码覆盖率统计
* GO提供的 race detector 能非常方便的进行并发编程
* 自己编写部分测试工具
* 移植有什么挑战
    * 两种语言对字符串的处理不同
    * 两种语言对异常的处理不同
    * GO不支持范型
    * GO不支持方法重载
    * GO没有继承
    * 接口处理不同
    * GO不支持包之间的循环导入
    * GO对访问级别不敏感（public，protected，private）
    * GO对并发的支持更友好
    * Java链式编程异常中断，GO需要使用 "stateful error"
    * GO 内置对JSON的支持
    * GO代码更简练
* 对组织工作对想法
    * 推荐 [Notion](https://www.notion.so/)

# Tips
IDEA 快捷键
https://github.com/haymaicc/idea-keymap/blob/master/keymap-windows.md

# Share
编程范式是指程序的编写模式，与具体的语言关系相对较小。目前只有三种编程范式，他们的目的是限制和规范程序员的能力，
没有一个范式是增加新能力的。也就是说编程范式的目的是设置限制，告诉我们不能做什么，而不是可以做什么。

## 结构化编程

结构化编程对程序的控制权的直接转移进行了限制和规范。

结构化编程让我们可以编写可证伪程序单元的能力。

## 面向对象编程
面向对象编程对程序控制权的间接转移进行了限制和规范。

### 封装
很难说封装是面向对象编程的必要条件，事实上，很多编程语言对封装性没有强制性要求（ python , Smalltalk , Javascript , lua , Ruby ）。

面向对象确实要求程序员尽量避免破坏数据的封装性，但事实上那些声称自己提供面向对象编程的语言，相对于 C 这种完美封装的语言，其封装性还被削弱了。

### 继承
面向对象并没有提供很好的封装性，在继承方面其实也一般。

继承的主要作用是让我们可以在某个作用域内对外部定义的某一组变量和函数进行覆盖。早在面向对象理论被提出之前，对继承的支持就已经存在很久了。

面向对象的编程语言对类型的向上转换隐形支持，可以得出面向对象在继承性方面虽然没有开创出新，但的确在数据结构的伪装性上提供了相当程度的便利性。

### 多态

在面向对象以前，所使用的语言也支持多态。多态其实不过是函数指针的一种应用，用函数指针显示实现多态的问题在于函数指针的危险性，而面向对象消除了
这方面的危险性，采用面向对象让多态实现变得非常简单。

### 面向对象到底是什么？
面向对象就是已多态为手段来对源代码中的依赖关系进行控制的能力，这种能力让软件架构师可以构建出某种插件式架构，让高层策略性组件与底层实现性组件
相分离，底层组件可以被编译成插件，实现独立与高层组件的开发和部署。

## 函数式编程
函数式编程对程序中的赋值进行了限制和规范。

### 不可变性与软件架构
所有的竞争问题，死锁问题，并发更新问题都是可变变量导致的。如果变量永远不会被更改，那就不可能产生竞争或者并发更新问题。

### 可变性的隔离
一个架构设计良好的应用程序应该将状态修改的部分和不需要修改的部分隔离成单独的组件，然后用合适的机制来保护不可变变量。

软件架构师应该着力于将大部分处理逻辑都归于不可变组件中，可变组件的逻辑应该越少越好。

### 事件溯源
不保存状态，仅保存事务日志，需要状态，我们只需从头开始计算所有事务即可。代码版本管理工具就是使用这种方式。
