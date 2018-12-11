## 关于数组的一些运用

数组是在code中经常遇到的一些结构，js中许多的处理数组的方法，而不是只有for循环，所以code中更注重语义性，做什么事情用什么方法。

### 下列的例子统一用一个demo数组去处理
```js
let list = [
    {   
        id: 1,
        symbol: 'ETH/BTC',
        targetSymbol: 'ETH',
        marketSymbol: 'BTC'
    },
    {   
        id: 2,
        symbol: 'LTC/BTC',
        targetSymbol: 'LTC',
        marketSymbol: 'BTC'
    },
    {   
        id: 3,
        symbol: 'ETH/USDT',
        targetSymbol: 'ETH',
        marketSymbol: 'USDT'
    }
]
```

### for,forEach,map
可以用forEach去替代for循环，比如现在要对数组的每一个对象新增一个属性是否关注默认false,
用forEach取代for，不用说性能至少从语义行上是更好的，map和forEach的区别是map会返回一个新的数组，
所以就对当前数组进行操作的话forEach会是更的选择

```js
for(let i === 0; i <= list.length; i++) {
    list[i]['is_focus'] = false;
}

list.forEach(v => {
    v['is_focus'] = false;
})

list.map(v => {
    v['is_focus'] = false;
})

```

### filter,find,some
这三个方法都是用来过滤数组的,具体的不同在于他们的返回值不同，分别以不用的业务需求来运用它们

filter返回值数组
find返回值对象
some返回值布尔值

1.后台把所有的币对进行返回，现在我要根据不同的市场币显示不同的列表

```js
<!-- btc市场对 -->
list.filter(v => {
    return v['marketSymbol'] === 'BTC'
})
<!-- usdt实仓对 -->
list.filter(v => {
    return v['marketSymbol'] === 'USDT'
})
```

2.默认有一个币对需要检测是否在后台返回的数组里面

```js
const default_symbol = 'ETH/BTC'
const is_bool = list.some(v => {
    return v['symbol'] === default_symbol
})
// is_bool true
const obj = list.find(v => {
    return v['symbol'] === default_symbol
})
// obj = { id: 1, symbol: 'ETH/BTC', targetSymbol: 'ETH', marketSymbol: 'BTC'}
```
