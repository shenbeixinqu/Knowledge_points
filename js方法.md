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

