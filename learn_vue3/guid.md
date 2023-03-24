# Vue3部分
*   定义全局属性 app.config.globalProperties（这是对 Vue 2 中 `Vue.prototype` 使用方式的一种替代，此写法在 Vue 3 已经不存在了）使用方法如下：

```javascript
// 注入一个全局可用的 $translate() 方法
app.config.globalProperties.$translate = (key) => {
  // 获取 `options` 对象的深层属性
  // 使用 `key` 作为索引
  return key.split('.').reduce((o, i) => {
      if (o) return o[i]
  }, options)
}
```

使用全局定义的方法：

```javascript
import {getCurrentInstance } from 'vue'
const { ctx, proxy } = getCurrentInstance()  // 注意：ctx和proxy都可以访问到定义的全局方法，但是ctx只能在本地使用，线上环境使用proxy
proxy.$translate('哈哈')

// 在template中可以直接写方法名
<template>{{$translate('哈哈')}}</template>
```

*   ref() (接受一个内部值，这个值可以是基本数据类型也可以是一个复杂数据类型，返回一个响应式的、可更改的 refImpl 对象)

    reactive() (接受一个值(只能是对象或者是数据)返回一个对象的响应式代理)的使用如下：

```javascript
// ref的使用
// tips: 在setup语法糖中可以直接使用$ref()  在赋值的时候不需要.value来赋值
const username = ref('')
username.value = '张三'


// reactive的使用
// 在setup语法糖中同样具有简写形式（详情见文档）
const o = reactive({
  name:'Cooper',
  age:18
})
// 注意：不能给o直接赋值另外一个对象 否则会丢失响应式

// 解决方法
// 方法一：
const o = reactive({
  info:{
	name:'Cooper',
  	age:18
  }
})
o.info = {
  name:'lisi'
}
// 方法二 可以用ref来定义对象（推荐）


// 另外还要注意：不能直接解构定义的对象 否则会丢失响应式
const state = reactive({
    username:'zs'
    password:'1111'
})
let {username,password} = state

// 解决方法 这样就不会丢失响应式
let {username,password} = toRefs(state)
```

**// 注意：在模板中使用是不用带.value  原因 ref 在模板中使用的时候会自动解包(详情见文档)**

*   组件Props的声明和使用

```javascript
// 不需要手动导入 defineProps
const props = defineProps({
	propsName:{
       type:String,
       default:'测试'
    }
})

// 使用(在模板中使用时和vue2一样)
console.log(props.propsName)
```

*   ref 属性的使用

```javascript
// ref 它允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用
<script setup>
import { ref } from 'vue'
const p = ref()
</script>

<template>
  <p ref="p">hello</p>
</template>

//20230314 详解  上面的例子可以改写
<script setup>
import { ref } from 'vue'
const p = ref()
</script>

<template>
  <p :ref="(el)=>p = el">hello</p>
</template>

```

**注意:如果通过ref来获取某个组件中的属性个方法 该组件必须通过 defineExpose() 来导出 详情可见文档 <https://cn.vuejs.org/api/sfc-script-setup.html#defineexpose>**

*   watch的使用

```javascript
import {watch} from 'vue'
watch(()=>xxx,()=>{})
watch(xxx,()=>{})  // xxx 必须是ref对象或者是响应式对象
watch([xxx,yyy],()=>{})
// 注意，你不能直接侦听响应式对象的属性值  你应该这个样写
watch(()=>xxx.sss,()=>{})
```

**注意：监测复杂类型或者是数组时 vue内部会隐式地创建一个深层侦听器，但是一个返回响应式对象的 getter 函数，只有在返回不同的对象时，才会触发回调此时应该为其添加deep\:true参数 具体见文档 <https://cn.vuejs.org/guide/essentials/watchers.html#deep-watchers>**

&#x20;**更多参数和用法请见文档  <https://cn.vuejs.org/api/reactivity-core.html#watch>**

*   自定义事件

```javascript
// 定义事件
const emit = defineEmits(['change', 'delete'])
// 使用
emit('change')

// 传参
const emit = defineEmits({
  change(xxx){
	return true
  }
})
emit('change',xxx)
```

*   组件懒加载

```javascript
import {defineAsyncComponent} from 'vue'
const Header = defineAsyncComponent(() => import('@/components/Header.vue'))
```