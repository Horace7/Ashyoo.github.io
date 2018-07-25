---
layout: post
title: 'JavaScript中常用的的方法'
subtitle: '整理一下敲一敲常用的js方法'
date: 2017-08-10
tags: JavaScript
catalog: true
---

平常的项目中用到数组的地方挺多的,基本上从后台拿到的json数据不是对象就是数组

好记性不如我这24k氪金键盘啊,所以这里整理一下敲一敲常用的数组的操作<br>

不积跬步，无以至千里 加油

---

### concat
`concat 会合并两个数组`

```javascript
const arr = [5, 17, 6, 8]
const arr1 = [5, 17, 6, 8, 6, 8]

arr.concat(arr1)
// [5, 17, 6, 8, 5, 17, 6, 8, 6, 8]
```

### replace
`replace 用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。`

```javascript
// 表单验证需要判断20个字符,一个汉字等于2个字符
// 正则匹配所有的汉字,然后替换成字符,可以随意定义
value.replace(/[^/x00-\xff]/g, '**').length > 20
```

### splice
`splice 通过删除现有元素和/或添加新元素来更改一个数组的内容。`
```javascript
// splice可接收多个参数。第一个参数表示数组起始位置，第二个参数表示需要删除元素的个数
// 如果后面还有参数，则将随后的全部参数插入到第一个参数表示的起始位置。

[1,2,3,4,5].splice(3,1,"test1","test2") // [4] 返回被删除的元素组成的数组

// 此句代码表示从arr数组的第3个元素开始，删除随后1个元素，并将“test1”和“test2”插入到第3个元素之后。
```

### split
`split 使用指定的分隔符字符串将一个String对象分割成 字符串数组`

### substr
`substr 返回一个字符串中从指定位置开始到指定字符数的字符。`
> 接受两个参数 substr(start, length)
> 第一个参数表示开始位置的下标,第二个参数(可选)表示提取的字符数

```js
(function parseQueryString(url) {
  var json = {};
  var arr = url.substr(url.indexOf('?') + 1).split('&');
  // [key0 = 0, key1 = 1, key2 = 2]
  arr.forEach(function(item) {
    var tmp = item.split('=');
    json[tmp[0]] = tmp[1];
  })
  console.log(json) // {key0: "0", key1: "1", key2: "2"}
  return json;
}) ('http://shihaoran.top/index.html?key0=0&key1=1&key2=2')
```

### some
`匹配数组中每一个元素,如果有一个元素通过由提供的函数实现的测试,则立刻返回true,否则返回false`

```javascript
  // 可以用到的地方就很多了,比如需要判断返回的数据中存不存在某个元素,就可以用这个方法来判断
const arr = [5, 17, 6, 8]

arr.some(e => { return  e > 9 }) // true
```

### every
`匹配数组中每一个元素,如果所有元素都通过由提供的函数实现的测试,则返回true,有一个没有通过则返回false`
```javascript
const arr = [5, 17, 6, 8]

arr.every(e => { return  e > 9 }) // false
```

### filter
`筛选出数组中符合条件的元素,组成一个新的数组`
```javascript
const arr = [5, 17, 6, 8]

// 可以过滤出想要的元素再进行操作
arr.concat(arr1).filter(e =>  e > 7) // [17, 8]

const arr1 = [ 'name1', 'name2', 'name3', 'name2' ]

// filter 可以接收3个参数 第一个参数表示数组中的元素,另外两个参数表示元素的位置和数组本身
// 利用这3个参数,可以进行数组去重
arr.filter((item, index, self) => {
  // indexOf 总是返回第一个元素的位置 后续的重复元素位置与indexOf返回的位置不相等，因此被filter滤掉了。
  return self.indexOf(item) === index
})

// [ 'name1', 'name2', 'name3' ]
```

### map
`数组中的每个元素都调用一个提供的函数后返回一个新数组`
```javascript
const kvArr = [
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 2, value: 20},
    {key: 3, value: 30},
    {key: 4, value: 30},
]

kvArr.map(item => item.key).filter(e => e > 1) //  [2, 3, 4]
```

### forEach
`让数组中的每一项做一件事`
```javascript
const kvArr = [
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 2, value: 20},
    {key: 3, value: 30},
    {key: 4, value: 30},
]

kvArr.forEach(item => console.log(item))
// {key: 1, value: 10}
// {key: 1, value: 10}
// {key: 1, value: 10}
// {key: 2, value: 20}
// {key: 3, value: 30}
// {key: 4, value: 30}
```

### push *改变原数组*
`将一个或多个元素添加到数组的末尾，并返回新数组的长度`
```javascript
const arr = [5, 17, 6, 8]

arr.push(4) // 5
```

###  includes
`方法用来判断一个数组是否包含一个指定的值,返回 true或 false`
```javascript
const arr = [5, 17, 6, 8]

arr.includes(5) true
```

### join
`将数组（或一个类数组对象）的所有元素连接到一个字符串中`
```javascript
const kvArr = [
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 2, value: 20},
    {key: 3, value: 30},
    {key: 4, value: 30},
]

kvArr.map(item => {return item.key}).join(',')
//1,1,1,2,3,4
```

### reduce
`从左到右为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。`
```javascript
// 上次写的一个综合性的应用,从数组中取出所有的businessType并且去重
const shopInfo = [{
  shopName: '111',
  businessType: '1'
}, {
  shopName: '222',
  businessType: '2'
}, {
   shopName: '333',
   businessType: '2'
 }]
const businessTypeList = [...new Set(shopInfo.map(item => item.businessType).reduce((x, y) => x.concat(y)))]
```

### Object.assign()
`ES6中提供了Object.assign() ，用于合并/复制对象的属性。`

```javascript
const obj1 = {z: z}
const saveArgs = Object.assign({}, obj1, {
  x: x,
  y: y
})
 // 花括号叫目标对象，后面的obj1等等是源对象。对象合并是指：将源对象里面的属性添加到目标对象中去，若两者的属性名有冲突，后面的将会覆盖前面的
```

---

### 数组去重

* 第一种方法

```javascript
// new Set 中的元素只可以出现一次,返回一个新的Set对象
// Array.from()再从一个类似数组或可迭代对象中创建一个新的数组实例
Array.from(new Set(kvArr.map(item => item.key)))

//可以用es6的 ... 的解构赋值
[...new Set(kvArr.map(item => item.key))]
```
* 第二种方法

```javascript
// 定义一个空数组 let ret = []
// include判断是否包含元素
// push 如果不包含,则push到ret
let ret = []
kvArr.map(item => item.key).map(e => {
    if (!ret.includes(e)) {
      ret.push(e)
      return ret
    }
})
ret.map(a => {
    switch(a) {
        case 1 :
          return console.log(111)
        case 2 :
          return console.log(222)
    }
})
```

### 数组求和
* 这里要先明白一点
> JavaScript数组方法是特意定义为通用的，因此他们不仅应用在真正的数组,而且在类数组(包括String)对象上也能正确工作，
但是类数组对🐘无法继承自Array.prototype,所以可以通过Function.call间接地调用。

```js
// arguments对象 包含传递给函数的每个参数
// call的第一个参数表示真正调用reduce的环境变为了arguments对象
// 也就是reduce方法中的this是指向arguments的。所以就好像arguments也具有了数组的方法。
// 能调用call的只有方法，所以不能用[].call这种形式，得用[].reduce.call
function sum(){
    return [].reduce.call(arguments,function(x,b){
         return (x|0)+(b|0);
    })
}
sum(1, true, 'a', 'D', 1, 'F', 1, 'w')
// 4
```

### 对象转数组
* 可以用Object.keys

```javascript
const obj = { a: 1, b: 2 }

Object.keys(obj).map(key => { return { key: key, value: obj[key] }})
```

### 数组排序
* 可以用sort()方法 *改变原数组*
> 需要注意的是：sort()方法默认排序顺序是根据字符串Unicode码点。所以需要用函数表达式来实现数组的排序

```javascript
[2,51,11,62,91,4,4,444,33,5].sort((x,y) => x-y)
// [2, 4, 4, 5, 11, 33, 51, 62, 91, 444]

// sort也可以根据数组对象中的某一个字段排序
const obj = [{
  position: '4'
},{
  position: '999'
},{
   position: '1'
}]
obj.sort((x, y) => x.position - y.position)

// [{ position: '1' }, { position: '4' }, { position: '999' }]
```


