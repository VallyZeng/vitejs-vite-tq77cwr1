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
</template>