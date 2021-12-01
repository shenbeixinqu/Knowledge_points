





## computed/methods 

```shell
区别:
	computed是属性调用,methods是函数调用
	computed带有缓存功能,methods没有
```

## vuex

### state

```shell
state相当于组件中的data，用来存放全局的数据
使用：
	模板中 {{$store.state.name}}
	方法中 this.$store.state.name
```

**store/index.js**

```html
export default new Vuex.Store({
  // state相当于组件中的data,用来存放全局的数据
  state: {
    num: 0,
  },
  // getters相当于组件中的computed,getter是全局的,computed是组件的
  getters: {},
  mutations: {},
  actions: {},
  modules: {},
});
```

**Home.vue**

```html
<template>
  <div class="home">
    <h2>Home页面的数字:{{ $store.state.num }}</h2>
  </div>
</template>
```

**about.vue**

```html
<template>
  <div class="about">
    <h2>about页面的数字:{{ num }}</h2>
  </div>
</template>

<script>

export default {
  computed: {
    num() {
      return this.$store.state.num
    }
  }
};
</script>
```



### getters

 getters相当于组件中的computed,getter是全局的,computed是组件的

**store/index**

```shell
  // state相当于组件中的data,用来存放全局的数据
  state: {
    num: 0,
  },
  // getters相当于组件中的computed,getter是全局的,computed是组件的
  getters: {
    getNum(state) {
      return state.num
    }
  },
  mutations: {},
  actions: {},
  modules: {},
```

**Home.vue**

```shell
<template>
  <div class="home">
    <h2>Home页面的数字:{{ $store.getters.getNum }}</h2>
  </div>
</template>
```

### mutations

更改vuex的store中状态的唯一方法是提交mutation

**store/index**

```shell
// state相当于组件中的data,用来存放全局的数据
  state: {
    num: 0,
  },
  // getters相当于组件中的computed,getter是全局的,computed是组件的
  getters: {
    getNum(state) {
      return state.num
    }
  },
  // mutations相当于组件中的methods，但是不能使用异步方法，如定时器 axios 
  mutations: {
    increase(state, payload=1) {
      state.num += payload
    }
  },
  actions: {},
  modules: {},
```

**btn.vue**

```shell
<template>
  <div>
    <button @click="add">+1</button>
    <button @click="addFn">+5</button>
  </div>
</template>

<script>
export default {
  methods: {
    add() {
      this.$store.commit('increase')
    },
    addFn() {
      this.$store.commit('increase', 3)
    }
  }
};
</script>
```

### actions

**store/index**

```shell
 // state相当于组件中的data,用来存放全局的数据
  state: {
    num: 0,
  },
  // getters相当于组件中的computed,getter是全局的,computed是组件的
  getters: {
    getNum(state) {
      return state.num
    }
  },
  // mutations相当于组件中的methods，但是不能使用异步方法，如定时器 axios 
  mutations: {
    increase(state, payload=1) {
      state.num += payload
    },
    decrease(state) {
      state.num--
    }
  },
  // action 专门处理异步 实际修改状态值的还是mutations
  actions: {
    decreaseAsync(context) {
      setTimeout(() => {
        context.commit('decrease')
      }, 1000)
    }
  },
  modules: {},
```

**btn.vue**

```shell
<template>
  <div>
    <button @click="add">+1</button>
    <button @click="addFn">+5</button>
    <button @click="sub">-1</button>
  </div>
</template>

<script>
export default {
  methods: {
    add() {
      this.$store.commit('increase')
    },
    addFn() {
      this.$store.commit('increase', 3)
    },
    sub() {
      this.$store.dispatch('decreaseAsync')
    }
  }
};
</script>
```

### 辅助函数

mapState,mapGetters都是写在computed中

#### mapState

**about.vue**

```shell
<template>
  <div class="about">
    <h2>about页面的数字:{{ num }}</h2>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  computed: {
    ...mapState(['num']),
  }
};
</script>

```

#### mapGetters

**home.vue**

```shell
<template>
  <div class="home">
    <h2>Home页面的数字:{{ getNum }}</h2>
  </div>
</template>

<script>
import { mapGetters} from 'vuex'
export default {
  computed: {
    ...mapGetters(['getNum'])
  }
};
</script>
```

下面两个展开运算符 写在methods中

#### mapMutations

#### mapActions

```shell
<template>
  <div>
    <button @click="increase()">+1</button>
    <button @click="increase(5)">+5</button>
    <button @click="decreaseAsync()">-1</button>
  </div>
</template>

<script>
import { mapMutations, mapActions } from 'vuex'
export default {
  methods: {
    // add() {
    //   this.$store.commit('increase')
    // },
    // addFn() {
    //   this.$store.commit('increase', 3)
    // },
    // sub() {
    //   this.$store.dispatch('decreaseAsync')
    // },
    ...mapMutations(['increase','decrease']),
    ...mapActions(['decreaseAsync'])
  }
};
</script>

<style></style>

```

#### module

```shell
store可以认为是一个主模块，下面可以分为很多子模块，写完导入主模块中，以user模块为例
user模块可以有state getters mutations actions
users的index.js文件中需要写入 namespaced: true
在组件中获取值的方法
	$store.state.users.name
在方法中获取值的方法
	methosd:{
		...mapMutations({
		   'changeName': 'users/changeName'
		})
	}
```



## vue-router

**路由中基础的概念**

```shell
#1 route 一条路由是单数
#2 routes 一组路由,把上面的每一条路由组合起来,形成一个数组
#3 router 是一个机制,相当于一个管理者来管理路由
#4 $route对象 ( 当前路由信息对象 )
  表示当前激活的路由的状态信息,包含了当前url解析得到的信息,含有url匹配到的路由记录
  1.$route.path
      字符串，对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"。
  2.$route.params
  	  一个key/value对象,包含了动态片段和全匹配片段
  	  如果没有路由参数,就是一个空对象
  3.$route.query
      一个key/value对象,表示url查询参数
      例如 对于路径 /foo?user=1 $route.query.user == 1
      如果没有查询参数,则是个空对象
  4.$route.hash
  	  当前路由的hash值,不带'#'如果没有hash 则是空字符串
  5.$route.fullPath
  	  完成解析后的URL,包含查询参数和hash的完整路径
 #5 $touter对象
     全局的路由实例,是router构造方法的实例
     在Vue实例内部,可以通过$router访问路由实例
     
     全局挂载路由实例
     Vue.use(VueRouter)
      
     路由实例方法
     // 字符串
     this.$router.push('home')
     // 对象
     this.$router.push({ path: 'home' })
     // 带查询参数,变成 /register?plan=123
     this.$router.push({path:'register', query:{plan:'123'}})
     push方法的跳转和<router-link :to=''>是等同的
     
     this.$router.go(-1)  // 后退
     
```

**传递参数的方式**

传递参数主要由两种类型:params和query

- params的类型

  配置路由格式:  /router/:id

  传递的方式: 在path后面跟上对应的值

  传递后形成的路径: /router/123, /router/abc

- query的类型

  配置路由格式: /router 普通配置

  传递的方式: 对象中使用query的key作为传递方式

  传递后形成的路径: /router?id=123, /router?id=abc

## vue-clipboard2

点击复制按钮,把输入框或指定的文本复制到粘贴板

```shell
# 1 安装
	npm install vue-clipboard2
# 2 main.js 引入
import VueClipboard from 'vue-clipboard2'
Vue.use(VueClipboard)
# 3 使用
	<template>
		<input v-model="location_lead_copy" type="text" style="display: none">
         <el-button
            v-clipboard:copy="location_lead_copy"
            v-clipboard:success="onCopy"
            v-clipboard:error="onError"
         >复制地区</el-button>
	</template>	
	methods: {
	  onCopy(e) {
        this.$message.success('内容已复制到剪切板！')
      },
      onError(e) {
        this.$message.error('抱歉，复制失败！')
      }
	}
```



## lodash

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



