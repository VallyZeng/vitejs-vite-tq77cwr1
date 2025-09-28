<script setup>
import { ref, useTemplateRef, onMounted, watchEffect } from 'vue';

//第一个参数必须与模板中的ref匹配
const input = useTemplateRef('my-input')

onMounted(() => {
 input.value.focus()
})

watchEffect(() => {
 if(input.value) {
  input.value.focus()
 } else {
  // 此时还未挂载，或此元素已被卸载
 }
})

const list = ref([
 1, 2, 3
])

const itemRefs = useTemplateRef('items')
onMounted(() => console.log(itemRefs.value))
onMounted(() => alert(itemRefs.value.map(i => i.textContext)))
</script>

<template>
 <h2>访问模板引用</h2>
 <input ref="my-input"/>
 <h2>组件上的ref</h2>
 <h2>v-if中的模板引用</h2>
 <ul>
  <li v-for="item in list" ref="items">
   {{ item }}
  </li>
 </ul>
</template>