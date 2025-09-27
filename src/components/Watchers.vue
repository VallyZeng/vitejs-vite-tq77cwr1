<script setup>
import { ref, watch } from 'vue'

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

</script>

<template>
<h2>基本示例</h2>
<p>
 Ask a yes/no question:
 <input v-model="question" :disabled="loading" />
</p>
<p>{{ answer }}</p>

<h2>侦听数据源类型</h2>
</template>