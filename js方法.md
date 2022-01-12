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

