# design pattern

## 设计模式有多少种类型 ?

- Creational: 关注于如何实例化一个对象, 或者实例化一组相互关联的对象
- Structural: 关注于对象的组合, 实体间如何进行关联 (如何构建一个软件组件)
- Behavioral: 关注于为对象间分配能力. (如何在软件组件之间 *定义行为*)

## Structural 与 Behavioral 有什么不同 ?

Behavioral 不仅仅指定了对象的结构, 还规划出了对象间进行消息通信的框架.

## Creational Design Pattern

## Structural Design Pattern

## Behavioral Design Pattern

### Observer (观察者模式)

#### 观察者模式的定义是什么 ?

定义对象之间的依赖, 当一个对象的状态发生改变, 与之有依赖的其他对象都会收到通知.

改变的那个对象叫 subject, subject 会维护一个依赖者列表, 称为 observers

当 subject 状态发生改变, 自动会调用 observers 注册在 subject 上的方法.

#### 观察者模式要解决什么问题 ?

1. 定义了对象间一对多的依赖, 并且避免强耦合
2. 当一个对象的状态发生改变, 其所依赖的多个对象自动进行更新
3. *一个对象* 可以通知 *多个对象*

#### 观察者模式描述

1. 定义一个 `Subject` 和 `Observer` 对象
2. 当 subject 状态改变, 所有注册在 subject 上的 observers 会自动被通知以及更新

#### Subject 和 Observer 的职责是什么 ?

Subject: 维护一个 observers 数组, 当自身状态发生改变, 调用所有 observers 的 `update()` 方法

Observer: 在 Subject 上注册与注销, 实现自己的 `update()` 方法

#### 代码举例说明一个观察者模式的例子 ?

```js

class Subject {
  constructor() {
    this.state = {};
    this.observers = [];
  }

  attach(observer) {
    this.observers.push(observer);
  }

  detach(observer) {
    const i = this.observers.findIndex(observer);
    if (i > -1) {
      this.observers.splice(i, 1);
    }
  }

  getState() {
    return this.state;
  }

  setState() {}

  notify() {
    this.observers.forEach(o => o.update());
  }
}

class Observer {
  constructor(subject) {
    this.subject = subject;
  }

  update(state) {}
}

class Observer1 extends Observer {
  update() {
    const state = this.subject.getState()
    console.log(state);
  }
}

const subject = new Subject();
const o1 = new Observer1(subject);
const o2 = new Observer1(subject);
subject.attach(o1);
subject.attach(o2);
subject.notify();
```

#### 观察者模式的 类图 与 时序图 ?

##### 类图

[![design-pattern-observer.png](https://i.postimg.cc/fb1sYG1R/design-pattern-observer.png)](https://postimg.cc/Z9PXSMTG)

##### 时序图

[![design-pattern-observer.png](https://i.postimg.cc/B6WWjx18/design-pattern-observer.png)](https://postimg.cc/R3Tp8Jd9)


