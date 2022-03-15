### 对象数组属性值拼接成字符串

```shell
var objs=[
    {id:1,name:'张三'},
    {id:2,name:'李四'},
    {id:3,name:'王五'},
    {id:4,name:'赵六'},
];

var idsStr = objs.map(function(obj,index){
    return obj.id;
}).join(",");

console.log(idsStr);
# 结果 1,2,3,4
```

### 生成随机字符串+时间戳

```shell
function randomString(len) {
    len = len || 32;
    let timestamp = new Date().getTime();
    console.log('timestamp', timestamp)
    /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
    let $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';    
    let maxPos = $chars.length;
    let randomStr = '';
    for (let i = 0; i < len; i++) {
    randomStr += $chars.charAt(Math.floor(Math.random() * maxPos));
    }
    let res = randomStr + timestamp
    console.log('res', res, res.length)
    return randomStr + timestamp;
    }
randomString(17)
```

### js将对象分成多个数组

```shell
//数据归类方法
function classify(arr, key) {
    let kind = []; //存放属性标识
    let newArr = []; //返回的数据
    arr.map((item) => {
    // 判断key是否存在，不存在则添加
    if (!kind.includes(item[key])) {
        kind.push(item[key]); //kind添加新标识
        newArr.push([]); //添加数组
    }
    let index = kind.indexOf(item[key]); //返回带有标识在kind内的下标，判断加入哪个数组
    newArr[index].push(item); //将对象存入数组
    });
    return newArr;
}
//测试数据
let testArr = [
    {name:"张三",age:"18",gender:"男"},
    {name:"李四",age:"22",gender:"男"},
    {name:"王五",age:"17",gender:"女"},
    {name:"刘丽",age:"18",gender:"女"},
    {name:"李磊",age:"22",gender:"男"},
    {name:"杨梅",age:"18",gender:"女"}
];

//调用方法，对象被判断的属性值相同，则在同个数组内
let sameType = [];
//单次调用，输出二维数组
sameType.push(classify(testArr,"age"));
console.log(sameType);
//循环调用，可判断多个属性，输出多维数组
classify(testArr,"age").map((item) => {
sameType.push(classify(item,"gender"))
});  
// console.log(sameType);
```

### 提取数字

```shell
前面带数字,后面非数字,可以直接用parseFloat()函数
var num1 = parseFloat("2.89元"); //num1 : 2.89
```

### json

**JSON.parse**

```shell
JSON 通常用于与服务端交换数据。
在接收服务器数据时一般是字符串。
我们可以使用 JSON.parse() 方法将数据转换为 JavaScript 对象。
 var obj = JSON.parse('{ "name":"runoob", "alexa":10000, "site":"www.runoob.com" }');
 console.log('obj', obj, typeof obj)
 # obj {name: 'runoob', alexa: 10000, site: 'www.runoob.com'} object
```



**JSON.stringify**

```shell
JSON 的常规用途是同 web 服务器进行数据交换。
在向 web 服务器发送数据时，数据必须是字符串。
通过 JSON.stringify() 把 JavaScript 对象转换为字符串。

 var obj = { name:"Bill Gates", age:62, city:"Seattle"};
 var res = JSON.stringify(obj)
 console.log(obj, 'res', res, typeof res)
 # {name: 'Bill Gates', age: 62, city: 'Seattle'}
   'res'
   '{"name":"Bill Gates","age":62,"city":"Seattle"}'
   'string'
```

### Object.assign

**基本用法**

```shell
# Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
const target = { a: 1 };
const source1 = { b: 2 };
const source2 = { c: 3 };
Object.assign方法的第一个参数是目标对象，后面的参数都是源对象。

# 注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
# 如果只有一个参数，Object.assign会直接返回该参数。
const obj = {a: 1};
Object.assign(obj) === obj // true
# 如果该参数不是对象，则会先转成对象，然后返回。
typeof Object.assign(2) // "object"
# 由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。
Object.assign(undefined) // 报错
Object.assign(null) // 报错
# 如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果undefined和null不在首参数，就不会报错。
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true
```

**注意点**

```shell
# 同名属性的替换
对于这种嵌套的对象，一旦遇到同名属性，Object.assign的处理方法是替换，而不是添加。
const target = { a: { b: 'c', d: 'e' } }
const source = { a: { b: 'hello' } }
Object.assign(target, source)
// { a: { b: 'hello' } }
上面代码中，target对象的a属性被source对象的a属性整个替换掉了，而不会得到{ a: { b: 'hello', d: 'e' } }的结果。这通常不是开发者想要的，需要特别小心。

一些函数库提供 Object.assign的定制版本（比如 Lodash 的_.defaultsDeep方法），可以得到深拷贝的合并。
# 数组的处理
Object.assign可以用来处理数组，但是会把数组视为对象。
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
上面代码中，Object.assign把数组视为属性名为 0、1、2 的对象，因此源数组的 0 号属性4覆盖了目标数组的 0 号属性1。
```

**常见用途**

```shell
# 为对象添加属性
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
上面方法通过Object.assign方法，将x属性和y属性添加到Point类的对象实例。
# 为对象添加方法
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
});

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};
上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用assign方法添加到SomeClass.prototype之中。
# 合并多个对象
```

### reduce

**语法**

```shell
arr.reduce(callback, [initialValue])
```

```shell
reduce为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素
接受四个参数 初始值（或者上一次回调函数的返回值）, 当前元素值, 当前索引,调用reduce的数组
```

**eg1**

```js
var arr = [1, 2, 3, 4];
var sum = arr.reduce(function(prev, cur, index, arr) {
    console.log(prev, cur, index);
    return prev + cur;
})
console.log(arr, sum);
# 打印结果
1 2 1
3 3 2
6 4 3
[1,2,3,4] 10

上面例子index是从1开始的，第一次的prev的值是数组的第一个值。数组长度是4，但是reduce函数循环3次。
```

**eg2**

```js
var  arr = [1, 2, 3, 4];
var sum = arr.reduce(function(prev, cur, index, arr) {
    console.log(prev, cur, index);
    return prev + cur;
}，0) //注意这里设置了初始值
console.log(arr, sum);
# 打印结果
0 1 0
1 2 1
3 3 2
6 4 3
[1,2,3,4] 10

这个例子index是从0开始的，第一次的prev的值是我们设置的初始值0，数组长度是4，reduce函数循环4次。
```

**eg3**

```js
# 如果数组为空 运用reduce是什么情况
var  arr = [];
var sum = arr.reduce(function(prev, cur, index, arr) {
    console.log(prev, cur, index);
    return prev + cur;
})
//报错，"TypeError: Reduce of empty array with no initial value"

# 但是设置了初始值就不会报错
var  arr = [];
var sum = arr.reduce(function(prev, cur, index, arr) {
    console.log(prev, cur, index);
    return prev + cur;
}，0)
console.log(arr, sum); // [] 0
```

**简单用法**

```js
# 数组求和 求乘积
var  arr = [1, 2, 3, 4];
var sum = arr.reduce((x,y)=>x+y)
var mul = arr.reduce((x,y)=>x*y)
console.log( sum ); //求和，10
console.log( mul ); //求乘积，24
```

**高级用法**

```js
# 计算数组中每个元素出现的次数
let names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];
let nameNum = names.reducr((pre, cur) => {
    if(cur in pre) {
        pre[cur]++
    } else {
        pre[cur] = 1
    }
    return pre
})

# 将二维数组转化为一维
  let arr = [[1,2],[3,4],[5,6]]
  let newArr = arr.reduce((pre,cur) => {
      return pre.concat(cur)
  }, [])
  console.log('newarr', newArr)


# 对象里的属性求和
 var result = [
            {
                subject: 'math',
                score: 10
            },
            {
                subject: 'chinese',
                score: 20
            },
            {
                subject: 'english',
                score: 30
            }
        ]

        var sum = result.reduce((pre, cur) => {
            return pre + cur.score
        }, 0)
        console.log('sum', sum)   // 60
```

