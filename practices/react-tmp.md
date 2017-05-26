[TOC]

# [概念](https://github.com/dvajs/dva/blob/master/docs/Concepts_zh-CN.md)

Modles

State

> state表示model的状态数据，通常表现为一个javascript对象，当然它可以是任意值;操作的时候每次都要当作不可变数据来对待，保证每次都是全新对象，没有引用关系，这样才能保证state的独立性，便于测试和追踪变化。

```javascript
type State = any
const app = dva();
console.log(app._store);
```

Action

> Action是一个普通javascript对象，它是改变State的唯一途径。无论是从UI事件，网络回调，还是WebSocket等数据源所获得的数据，最终都会通过dispatch函数调用一个action,从而改变对应的数据。action必须带有type属性指明具体行迹，其它字段可以自定义，如果要发起一个action需要使用dispatch函数，需要注意的是dispatch是在组件connect Models以后，通过props传入的

```javascript
type AsyncAction = any
dispatch({
  type: 'add',
})

```

> dispatching function是一个用于触发action的函数，action是改变state的唯一途径，但是它只描述了一个行为，而dispatch可以看作是触发这个行为的方式，而reducer则是描述如何改变数据的。
>
> 在dva中，connect model的组件通过props可以访问到dispatch,可以调用model中的reducer或者effects,常见的形式如

Reducer

```javascript
type Reducer<S, A> = (State: S, action: A) => S

```

Reducer也称reducing function, 函数接受两个参数： 之前已经累积运算的结果和当前要被累积的值，返回的是一个新的累积结果，该函数把一个集合归并成一个单值

Reducer的概念来源于函数式编程，很多语言都有reduce api,如在javascript中

```javascript
[{x:1}, {y:2}, {z:3}].reduce(function(prev, next) {
  return Object.assign(prev, next);
})
```

