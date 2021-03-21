
# 面向对象

### 接口 和 类 的区别 ?

todo

### 类图 与 时序图 区别 ?

todo

### 类图

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




