key a value 1
index.vue?6ced:35 key b value 2lodash

lodash 的所有函数都不会咋原有的数据上进行操作,而是复制一个新的数据而不改变原有数据

### 数组类方法

#### 数组分割

**chunk**

将数组Array分成多个size长度的区块,并将这些区块组成一个新数组,如果Array无法分割成全部等长的区块,那么最后剩余的元素将组成一个区块

```shell
_.chunk(['a', 'b', 'c', 'd'], 2)
// => [['a', 'b'], ['c', 'd']]
```

#### 过滤非真值

**compact**

创建一个新数组,包含原数组中的所有非假值元素, 例如false,null,0,'',undefined和NaN都被认定是假值

```shell
_.compact([0,1,false,2,'',3])
// => [1,2,3]
```

#### 值与数组连接

**concat**

创建一个新数组,将array与任何数组或值连接在一起

```shell
var array = [1]
var other = _.concat(array, 2, [3], [[4]])
// other =>  [1, 2, 3, [4]]
```

#### 数组切片

**drop**

创建 一个切片数组,去除array前面的n个元素(n默认值为1)

```shell
_.drop([1,2,3])
// => [2,3]
_.drop([1,2,3], 2)
// => [3]
_.drop([1,2,3], 5)
// => []
```

**dropRight**

创建一个切片数组,去除array尾部的n个元素(n默认值为1)

```shell
_.dropRight([1,2,3])
// => [1,2]
_.dropRight([1,2,3],2)
// => [1]
```

#### 移除指定元素值

**pull**

移除数组中所有和给定值相等的元素

```shell
var arr = [1,2,3,1,2,3]
_.pull(arr,2,3)
// => [1,1]
```

**pullAll**

不同于pull的是,该方法接受一个要移除值的数组

```shell
var arr = [1,2,3,1,2,3]
_.pullAll(arr,[1,2])
// => [3,3]
```

#### 数组去重

**uniq**

创建一个去重后的array数组副本,只有第一次出现的才保留

```shell
_.uniq([2,1,2])
// => [2,1]
```

### 集合类方法

#### 遍历集合

**forEach**

```_
 _.forEach({ 'a': 1, 'b': 2 }, function(value, key) {
      console.log('key', key, 'value', value)
 })
 // => key a  value 1
       key b value 2
```

#### 遍历并返回映射

**map**

```shell
function square(n) {
	return n * n
}
_.map([4,8], square)
// => [16, 64]

_.map({ 'a': 4, 'b': 8 }, square)
// => [16, 64]

_.map([
        { 'user': 'barney' },
        { 'user': 'fred' }
      ], 'user')
// => ['barney', 'fred']
```

#### 返回集合长度

**size**



```shell
_.size(1,2,3)
// => 3

_size({'a':1, 'b':2})
// => 2

_size('pebbles')
// => 7
```



