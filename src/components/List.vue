<script setup>
import { ref } from 'vue';

const items = ([{message: 'Foo'}, {message: 'Bar' }])
// 访问父属性
const parentMessage = ref('Parent')

const myObject = ({
 title: 'How to do lists in Vue',
 author: "Jone doe"
})

</script>

<template>
<li v-for="item in items">
 {{ item.message }}
</li>
// 使用索引，访问父属性
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
 // 渲染包含多个元素的块
 <template v-for="item in items">
  <li>{{ item.message }}</li>
  <li class="divider" role="presentation">content</li>
 </template>
</ul>

<p>v-if 和 v-for</p>
v-if会比v-for又更高的优先级

</template>