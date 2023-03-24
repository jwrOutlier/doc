# Vue3+Ts

*   ref()和reactive() ts写法

```typescript
interface IO{
  name:string
  age:number
}
const o:IO = reactive({
  name:'Cooper',
  age:18
})
const o = ref<IO>({
  name:'Cooper',
  age:18
})
```

注意 不推荐使用 reactive() 的泛型参数，因为处理了深层次 ref 解包的返回值与泛型参数的类型不同  <https://cn.vuejs.org/guide/typescript/composition-api.html#typing-reactive>

*   Props Ts写法

```typescript
// 写法一
const props = defineProps({
	name:{
	  type:String as PropType<string>,
	  default:''
   }
})

// 写法二
interface IProps {
   name:string
}
const props = defineProps<IProps>()  // 此方法不能定义默认值
const props = withDefaults(defineProps<IProps>(),{
   name:'张三'
})

```

注意：在给props 定义类型时只能将类型定义在vue文件中 不能将它抽出去 否则vue会报错&#x20;

问题：[https://github.com/vuejs/core/issues/4294](\[https://github.com/vuejs/core/issues/4294]\(\\\[https://github.com/vuejs/core/issues/4294]\\\(\\\)\\\&#x20;\)\&#x20;)&#x20;

&#x20;解决方法 参考 <https://juejin.cn/post/7190781993037070393>  // 只是将改写法 改为写法一

*   defineEmits TS 写法：

```typescript
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()

emit('change',id)
```

*   为事件处理函数标注类型

```typescript
<template>
  <input type="text" @change="handleChange" />
</template>
<script setup lang="ts">
  // event 默认为隐式类型 any
  function handleChange(event) {
    console.log(event.target.value) // 这里会出现问题
  }

  // 解决方法
  function handleChange(event: Event) {
    console.log((event.target as HTMLInputElement).value)
  }
</script>
```

*   当定义一个数组时  ref(\[])  reactive(\[])  往数组中push对象或者属性时  必须给数组定义类型  否则不能通过ts编译

```typescript
const arr = reactive([])
arr.push({
  name:'张三',
  age:18
})  // 编译器出错  会判断arr是一个never[] 类型

//解决方法
type T = {
 name:string,
 age:number
}
const arr:T = reactive([])
```

*   为 provide / inject 标注类型

```typescript
import { provide, inject } from 'vue'
import type { InjectionKey } from 'vue'

const key = Symbol() as InjectionKey<string>

provide(key, 'foo') // 若提供的是非字符串值会导致错误

const foo = inject<string>(key) // foo 的类型：string | undefined
```


