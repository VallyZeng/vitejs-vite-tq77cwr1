<script setup>
import { computed, ref } from 'vue';
import TodoItem from './TodoItem.vue'

const items = ([{message: 'Foo', id: 1, title: 'i am title'}, {message: 'Bar', id: 2, title: 'i am title2'}])
// 访问父属性
const parentMessage = ref('Parent')

const myObject = ({
 title: 'How to do lists in Vue',
 author: "Jone doe"
})

const todos = ([
 {
  name: "test",
  complete: true
 },
 {
  name: "learn vue",
  complete: false
 }
])

const numbers = ref([1, 2, 3]);
const eventNumber = computed(() => {
 return [...numbers.value].reverse()
})

const sets = ref([
    [1,2,3,4,5],
    [7,8,9,10,11]
])

function even(numbers) {
 return numbers.filter((number) => number % 2 === 0)
}
</script>

<template>
<li v-for="item in items">
 {{ item.message }}
</li>
使用索引，访问父属性
<li v-for="(item, index) in items">
 {{ parentMessage }} - {{ index }} - {{ item }}
</li>
<p>遍历对象的所有属性</p>

<ul>
 <li v-for="value in myObject">
  {{ value }}
 </li>
</ul>
<p>多参数，第二个参数是属性名，第三个参数是索引</p>
<ul>
 <li v-for="(value, key, index) in myObject">
  {{ value }} - {{ key }} - {{ index }}
 </li>
</ul>

<p>在v-for中使用范围值</p>
<span v-for="n in 3">{{ n }}</span>

<p>template 上的 v-for</p>
<ul>
 渲染包含多个元素的块
 <template v-for="item in items">
  <li>{{ item.message }}</li>
  <li class="divider" role="presentation">content</li>
 </template>
</ul>

<p>v-if 和 v-for</p>
v-if会比v-for又更高的优先级
<!--
这个写法会抛出一个异常,，因为对v-if来说，todo没有被定义，v-if优先级更高，所以他先被初始化
<li v-for="todo in todos" v-if="!todo.complete">
 {{ todo.name }}
</li>
-->
<template v-for="todo in todos">
<li v-if="!todo.complete">
{{ todo.name }}
</li>
</template>
<p>通过key管理状态，为每一个元素穿入一个唯一key</p>
<div v-for="item in items" :key="item.id">
 {{ item.message }}
</div>
template写法
<template v-for="item in items" :key:="item.id">
 <li>{{ item.message }}</li>
</template>
<p>组件上使用v-for</p>
通过传递 props，使组件能够拿到值
<TodoItem v-for="item in items" :key="item.id" :title="item.title"/>
<p>展示过滤或排序后的结果</p>
<ul>
 <li v-for="number in eventNumber">{{ number }}</li>
</ul>
多层嵌套的情况可使用函数操作
<ul v-for="numbers in sets">
 <li v-for="n in even(numbers)">{{ n }}</li>
</ul>
</template>