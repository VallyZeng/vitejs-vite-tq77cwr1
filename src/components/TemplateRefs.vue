<script setup>
import { ref, useTemplateRef, onMounted, watchEffect } from 'vue';
import Child from './Child.vue'
import BlogPost from './templateRefs/BlogPost.vue'
import AlertBox from './templateRefs/AlertBox.vue';
import Home from './templateRefs/Home.vue'
import Archive from './templateRefs/Archive.vue'
import Posts from './templateRefs/Posts.vue'


//第一个参数必须与模板中的ref匹配
const input = useTemplateRef('my-input')
const child = useTemplateRef('child')

onMounted(() => {
 input.value.focus()
})

onMounted(() => {
 console.log('child', child.value)
})

watchEffect(() => {
 // 初次渲染时，input为null，唯一要考虑到空值的情况
 if(input.value) {
  input.value.focus()
 } else {
  // 此时还未挂载，或此元素已被卸载
 }
})

watchEffect(() => {
 if(child.value) {
  console.log('child=>', child.value)
 }
})

const list = ref([
 1, 2, 3
])

const itemRefs = useTemplateRef('items')
onMounted(() => console.log(itemRefs.value))
onMounted(() => console.log(itemRefs.value.map(i => i.textContent)))
// 传递Props
const posts = ref([
 { id: 1, title: 'my journey with Vue'},
 { id: 2, title: 'Blogging with Vue'},
 { id: 3, title: 'Why vue is so fun'}
])
const postFontSize = ref(1)

// 动态组件
const currentTab = ref('Home')
const tabs = {
 Home,
 Archive,
 Posts
}
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
 <Child ref="child" />
 <!-- 传递Props -->
 <div :style="{ fontSize: postFontSize + 'em' }">
  <BlogPost v-for="post in posts" :key="post.id" :id="post.id" :title="post.title" 
       @enlarge-text="postFontSize += 0.1"/>
 </div>
 <!-- 通过插槽来分配内容 -->
 <AlertBox>
  Something bad happend.
 </AlertBox>
 <!-- 动态组件 -->
<div class="demo">
 <button
  v-for="(_, tab) in tabs" :key="tab" 
  :class="['tab-button', { active: currentTab === tab }]"
  @click="currentTab = tab"
 >
  {{ tab }}
 </button>
 <component :is="tabs[currentTab]" class="tab"></component>
</div>
</template>

<style>
.demo {
  font-family: sans-serif;
  border: 1px solid #eee;
  border-radius: 2px;
  padding: 20px 30px;
  margin-top: 1em;
  margin-bottom: 40px;
  user-select: none;
  overflow-x: auto;
}

.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #e0e0e0;
}
.tab-button.active {
  background: #e0e0e0;
}
.tab {
  border: 1px solid #ccc;
  padding: 10px;
}
</style>