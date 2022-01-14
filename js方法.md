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

