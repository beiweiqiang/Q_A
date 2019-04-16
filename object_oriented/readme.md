
# 面向对象

## object 和 class 有什么区别 ?

|     | object | class |
| --- |:---|:---|
| 1 | object 是 class 的实例 | class 是 object 的模板/蓝图 |
| 2 | object 是真实世界的实体 | class 是一组相似 object 的集合 |
| 3 | 物理实体 | 逻辑实体 |
| 4 | 通过 new 关键字进行创建 | 通过 class 关键字进行声明 |
| 5 | 可创建多次 | 只会声明一次 |
| 6 | 创建时会分配内存 | 声明时不会分配内存 |

## 什么是面向对象 ?

面向对象是一种程序开发的 **编程范式**

传统的面向过程: 针对函数进行编程, 可看作是给计算机下达一系列的指令

面向对象是基于 *对象* 的一种编程范式, 把程序看成是一系列互相作用的对象, 对象包含了 *数据*, 以及 *对数据的操作*

对象会将数据进行封装, 外部只能通过对象暴露出来的操作来改变对象的数据

面向对象的目标是: 提升程序的 *可扩展性* 和 *可维护性*.

## 面向对象 vs 面向过程 ?

- 面向对象软件开发基于 *模块*, 代码耦合性低
- 面向对象基于 *模块* 进行开发, 测试更简单

## 什么是 编程范式 ?

范式 指 模式, 方法. 编程范式指编程风格. 如: 函数式编程, 面向对象编程, 指令编程...
编程范式决定了程序员对程序执行的看法.
如, 面向对象编程中, 程序员认为程序是一系列互相作用的对象.
函数式编程中, 程序是无状态的函数计算的序列
不同的编程语言会提倡不同的编程范式, 如 Java 面向对象, Haskell 函数式编程

## 面向对象的特征是什么 ?

封装, 继承, 多态

## 什么是 封装 ?

利用抽象数据类型将 *数据* 和 *基于数据的操作* 封装在一起, 使其构成一个不可分割的独立实体.

数据被保护在抽象数据类型的内部, 尽可能地隐藏内部的细节, 只保留一些对外接口使之与外部发生联系. 用户无需知道对象内部的细节, 可以使用对象对外提供的接口来访问对象.

```js
class Person {
  constructor() {
    this.name = '';
  }

  setName(name) {
    this.name = name;
  }

  getName() {
    return this.name;
  }
}

const p = new Person();
p.setName('heanqi');
p.getName();
```

## 封装的优点 ?

- 降低耦合: 独立的开发, 测试, 调试
- 降低维护成本: 以模块为单元进行调试, 不影响其他模块
- 提高代码可复用性

## 什么是继承 ?

继承实现了 is-a 的关系, 子类对象可以替换父类对象.

比如: `Animal animal = new Cat()`

以上, `Cat` 可以当做 `Animal` 来使用, 也就是说使用 `Animal` 引用 `Cat` 对象.

*父类引用* 指向 *子类对象* 称为 *向上转型*.

继承是多态的条件.

## 继承的优点 ?

- 子类自动继承父类的接口
- 提高代码可复用性

## 什么是多态 ?

多态分为 *编译时多态* 和 *运行时多态*.
- 编译时多态: 方法的重载
- 运行时多态: 程序中定义的 *对象引用所指向的具体类型* 在运行期间才确定

多态 指 不同对象中同种行为的不同实现形式.

## 运行时多态 的三个条件 ?

- 继承
- 覆盖 (重写)
- 向上转型

eg:
```js
class Person {
  say() {
    console.log('person');
  }
}

class Student extends Person {
  say() {
    console.log('student');
  }
}

class Teacher extends Person {
  say() {
    console.log('teacher');
  }
}

const arr = [];
arr.push(new Person());
arr.push(new Student());
arr.push(new Teacher())
arr.forEach(i => i.say());
```

## 多态的优点 ?

- 扩展性

## 接口 和 类 的区别 ?

todo

## 类图 与 时序图 区别 ?

todo

## 类图

### 类图中有哪些关系 ?

#### 泛化关系 (Generalization)

描述继承的关系

[![29.png](https://i.postimg.cc/XY78nj3B/29.png)](https://postimg.cc/9rKTpjDc)

#### 实现关系 (Realization)

实现一个接口

[![30.png](https://i.postimg.cc/tTHnCkPv/30.png)](https://postimg.cc/jWZSMzMy)

#### 聚合关系 (Aggregation)

表示整体由部分组成, 但是整体与部分 *不是强依赖的*, 整体不存在了部分还是会继续存在

[![31.png](https://i.postimg.cc/PrGfGJWK/31.png)](https://postimg.cc/mhwWCb6F)

#### 组合关系 (Composition)

表示整体由部分组成, 但是整体与部分是 *强依赖的*, 整体不存在了部分也会不存在.

[![32.png](https://i.postimg.cc/x1TWBV4t/32.png)](https://postimg.cc/cr2982Zn)

#### 关联关系 (Association)

表示不同类之间有关联, 是一种静态关系, 与运行过程的状态无关, *最开始就确定*.

可以有 *1对1*, *多对1*, *多对多* 的关联关系.

eg: 学生和学校 是 多对1 的关联关系.

[![33.png](https://i.postimg.cc/vTsKzJn7/33.png)](https://postimg.cc/R6gGCbXq)

#### 依赖关系 (Dependency)

表示不同类之间有关联, 但是与关联关系不同, 依赖关系是在运行过程中起作用的.

[![34.png](https://i.postimg.cc/hGRVfKR0/34.png)](https://postimg.cc/NKbKnqMy)

### 类图相关的疑问 ?

#### 如何判断 A类 与 B类 是依赖关系:

- A类 是 B类方法 的局部变量
- A类 是 B类方法 当中的一个参数
- A类 向 B类 发送消息, 从而影响 B类 发生变化

#### 关联关系 与 依赖关系 代码实例 ?

todo

关联关系:

#### 关联(Association), 聚合(Aggregation), 组合(Composition) 的关系 ?

组合 和 聚合 是关联的两种形式.

[![Associatn.png](https://i.postimg.cc/JntRFbZn/Associatn.png)](https://postimg.cc/jw0Vwn80)

#### 如何区分聚合和组合 ?

聚合:
- 体现了 *Has-a* 的关系
- 是一种 *单向的关联*, 比如学校有学生, 不能说学生有学校
- 在现实中, 两个实体分开以后 *可以* 单独存活
- strong Association

组合:
- 体现了 *part-of* 的关系
- 在现实中, 两个实体分开以后 *不能* 单独存活
- weak Association

#### 怎样才能说两个对象有 关联(Association) ?

todo




