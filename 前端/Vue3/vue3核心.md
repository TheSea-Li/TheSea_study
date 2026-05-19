# 一、Vue3 单文件组件（SFC）标准结构
```vue
<template>
  <!-- 模板：只能有一个根节点 -->
  <div class="box">{{ msg }}</div>
</template>

<!-- setup语法糖 + TS（企业标准） -->
<script setup lang="ts">
// JS/TS 逻辑
</script>

<!-- scoped：样式只作用当前组件 -->
<style scoped>
.box { color: red; }
</style>
```

# 二、响应式核心 API（最重要）
1. ref（基础类型：数字 / 字符串 / 布尔）
```js
import { ref } from 'vue'
// 定义响应式变量
const count = ref(0)
// 修改值必须加 .value
const add = () => count.value++
```
模板中直接用：{{ count }}

2. reactive（对象 / 数组）
```js
import { reactive } from 'vue'
const user = reactive({
  name: '张三',
  age: 18
})
// 修改不用 .value
user.name = '李四'
```

# 3. toRefs（解构 reactive 不丢失响应式）
```js
import { toRefs } from 'vue'
const user = reactive({ name: '张三', age: 18 })
// 解构后仍是响应式
const { name, age } = toRefs(user)
```

# 三、模板常用指令
1. 插值：{{ 变量 }}
2. 属性绑定：v-bind:src 简写 :src
3. 事件绑定：v-on:click 简写 @click
4. 双向绑定：v-model（表单专用）
```vue
<input v-model="inputVal" />
```
5. 条件渲染
- v-if：销毁 / 创建（耗性能，少用）
- v-show：display 控制（频繁切换用）
6. 列表渲染：v-for（必须绑定 key）
```vue
<li v-for="item in list" :key="item.id">{{ item.name }}</li>
```

# 四、计算属性 computed（缓存数据）
依赖变化才重新计算，性能优化必备
```js
import { ref, computed } from 'vue'
const count = ref(10)
// 计算双倍值
const doubleCount = computed(() => count.value * 2)
```

五、监听属性 watch（数据变化监听）
```js
import { ref, watch } from 'vue'
const count = ref(0)

// 普通监听
watch(count, (newVal) => {
  console.log('count变了：', newVal)
})

// 深度监听对象
watch(user, (newVal) => {}, { deep: true })
```

六、组件通信（高频）
1. 父 → 子：defineProps（TS 版）
```vue
<!-- 子组件 Child.vue -->
<script setup lang="ts">
// 定义 Props 类型
const props = defineProps<{
  title: string
  list?: string[]
}>()
</script>
```
2. 子 → 父：defineEmits
```
<!-- 子组件 -->
<script setup lang="ts">
const emit = defineEmits<{
  sendMsg: [msg: string]
}>()
// 触发事件
const toParent = () => emit('sendMsg', '我是子组件数据')
</script>
```

3. 子组件暴露属性：defineExpose
```js
// 子组件暴露方法/属性
defineExpose({ childMsg: 'hello' })
```

# 七、生命周期（setup 版）
最常用 3 个：
```js
import { onMounted, onUpdated, onUnmounted } from 'vue'

// 组件挂载完成（请求数据）
onMounted(() => console.log('页面加载完成'))

// 组件更新
onUpdated(() => {})

// 组件销毁（清除定时器/事件）
onUnmounted(() => {})
```

# 八、Vue Router（路由）
1. **基础配置**
```js
import { createRouter, createWebHistory } from 'vue-router'
const routes = [
  { path: '/', component: () => import('@/views/Home.vue') }
]
const router = createRouter({
  history: createWebHistory(),
  routes
})
export default router
```

2. **路由跳转 / 参数**
```js
import { useRouter, useRoute } from 'vue-router'
const router = useRouter()
const route = useRoute()

// 跳转
router.push('/about')
// 获取参数
console.log(route.query.id)
```

# 九、Pinia（状态管理，替代 Vuex）
1. **定义 Store**
```js
// stores/user.ts
import { defineStore } from 'pinia'
export const useUserStore = defineStore('user', {
  state: () => ({ name: '张三', age: 18 }),
  getters: {
    doubleAge: state => state.age * 2
  },
  actions: {
    changeName(name: string) {
      this.name = name
    }
  }
})
```

2. **使用 Store**
```js
import { useUserStore } from '@/stores/user'
import { storeToRefs } from 'pinia'

const store = useUserStore()
// 解构保持响应式
const { name, age } = storeToRefs(store)
// 调用方法
store.changeName('李四')
```














































