<script setup>
import { reactive, ref, watch, watchEffect } from 'vue'
import CallbackFlushTimeing from './watchers/CallbackFlushTimeing.vue';

/**
在组合式 API 中，我们可以使用 watch 函数在每次响应式状态发生变化时触发回调函数 
*/

const question = ref('')
const answer = ref('Question usually contain a question mask. ;-)')
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {
 if(newQuestion.includes('?')) {
  loading.value = true
  answer.value = 'Thinking...'
  try {
   const res = await fetch('https://yesno.wtf/api')
   answer.value = (await res.json()).answer
  } catch(error) {
    answer.value = 'Error! Could no retch the API. ' + error
  } finally {
    loading.value = false
  }
 }
})

// 侦听数据源
// 可以是一个ref计算属性
const x = ref(0)
const y = ref(0)
// 单个ref
watch(x, (newX) => {
 console.log(`x is ${newX}`)
})

// getter函数
// get函数要用下面这种写法才行
watch(
 () => x.value + y.value,
 (sum) => {
  console.log(`sum of x + y is :${sum}`)
 }
)
const obj = reactive({count: 0})
// 错误的写法,不能直接侦听响应式对象的属性值
// watch(obj.count, (count) => console.log(`Count is ${count}`))
// 这里需要用一个返回改属性的getter函数
watch(()=> obj.count,
(count) => console.log(`Count is ${count}`))

// 深层侦听器
watch(obj, (newValue, oldValue) => {
  // 在嵌套的属性变更时触发
  // 注意：`newValue` 此处和 `oldValue` 是相等的
  // 因为它们是同一个对象！
  // count++的时候被调用
  // 我个人理解这里就是对象里的属性发生了变化，对象本身的引用还是同一个
  // 这里的newValue和oldValue就是obj对象，而不是count，请注意
  console.log(`deep>old:${oldValue.count},new:${newValue.count}`)
})
const deepObj1 = reactive({count:1});
const deepObj2 = reactive({count:2});
//那么如果对象里的属性是一个响应式对象咋办，
const stateObj = ref(deepObj1)
watch(() => stateObj.value, (newValue, oldValue) => {
  // 只有obj对象被替换成其他对象了，这里才会被触发
  console.log(`obj swi,cur oldValue:${oldValue.count},newValue:${newValue.count}`)
})

function swi(obj) {
  if(obj === deepObj1) {
    stateObj.value = deepObj2;
  } else {
    stateObj.value = deepObj1;
  }
}
// 强制转换成深层监听
watch(() => stateObj.value,
(newValue, oldValue)=> {
  console.log(`深度监听：obj swi,cur oldValue:${oldValue.count},newValue:${newValue.count}`)
},
// deep属性可以用数字替换，代表深层监听的层数
{ deep:true })
// 即时回调的侦听器
watch(() => stateObj.value, (newValue, oldValue) => {
 // 立即执行，且当监听的属性改变时再次执行
//  注意，首次执行，oldValue是没有值的
 console.log(`立即执行：obj swi,cur oldValue:${oldValue},newValue:${newValue.count}`)
},
{ immediate: true })

watch(() => stateObj.value, () => {
// 当属性发生变化时，执行一次，只会执行一次
 console.log(`我是一次性侦听器`)
},
{once : true})
// watchEffect
const todoId = ref(1)
const data = ref(null)

// watch(
//  () => todoId,
//  async() => {
//   const response = await fetch(
//    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
//   )
//   data.value = await response.json()
//   console.log(`todoId变动`)
//  },
// )
// 上面的代码用watchEffect替代, watchEffect可以注册需要监听的变量（除了异步调用中使用到的变量）
watchEffect(async () => {
  console.log("正在获取数据", `${todoId.value}`)
  try {
   const response =await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
   )
   data.value = await response.json()
  } catch(error) {
   console.error('获取数据失败：', error)
   data.value = {error:'获取失败'}
  }
})

// 副作用清理
const userId = ref(1)
const userData = ref(null)

watchEffect((onCleanup) => {
 const controller = new AbortController()
 const currentUserId = userId.value

 console.log(`获取用户 ${currentUserId} 的数据`)

 // 注册清理回调函数
 onCleanup(() => {
  console.log(`清理用户 ${currentUserId} 的数据`)
  controller.abort()
 })


 fetch(`https://jsonplaceholder.typicode.com/users/${currentUserId}`, {
  signal: controller.signal
 }).then(response => response.json())
 .then(data => {
  userData.value = data
  console.log(` 用户${currentUserId} 数据加载完成`)
 })
 .catch(error => {
  if(error.name !== 'AbordError') {
   console.error('请求错误', error)
  } else {
   console.log(`用户 ${currentUserId} 请求被取消`)
  }
 })
})
//回调的触发时机
// postWatch


</script>

<template>
<h2>基本示例</h2>
<p>
 Ask a yes/no question:
 <input v-model="question" :disabled="loading" />
</p>
<p>{{ answer }}</p>

<h2>单个ref和getter函数</h2>
<button @click="x++">触发X++</button>
<h2>侦听响应式对象</h2>
<button @click="obj.count++">obj.count++</button>
<h3>深层侦听器</h3>
<button @click="swi(stateObj)">切换响应式对象中的对象</button>
<h2>watch effect</h2>
<button @click="todoId++">获取下一个Dodo</button>
<button @click="todoId = 1">重置为1</button>
<div>{{ data }}</div>
<h2>副作用清理</h2>
<div>
 <h3>用户数据加载（带取消）</h3>
 <button v-for="i in 5" :key="i" @click="userId = i" :style="{margin: '5px'}">
 用户 {{ i }}
 </button>

 <div v-if="userData">
  <h4>用户信息：</h4>
  <p>姓名：{{ userData.name }}</p>
  <p>邮箱：{{ userData.email }}</p>
 </div>
</div>

<CallbackFlushTimeing />
</template>