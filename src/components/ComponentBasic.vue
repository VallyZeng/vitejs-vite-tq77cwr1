<script setup>
 import { ref } from 'vue'
 import ButtonCounter from './componentBasic/ButtonCounter.vue';
 import BlogPost from './componentBasic/BlogPost.vue';
 
 const posts = ref([
  {id: 1, title: 'My journey with vue'},
  {id: 2, title: 'test'}
 ])
 const postFontSize = ref(1)
 function enlargeText() {
  postFontSize += 0.1
 }
</script>

<template>
 <h1>Here is a child component!</h1>
 <!-- 每个组件都维护着自己的Count -->
 <ButtonCounter />
 <ButtonCounter />
 <ButtonCounter />
 <h1>传递Props</h1>
 <div :style="{ fontSize: postFontSize + 'em' }">
  <BlogPost title="My journey with vue" @enlarge-text="postFontSize += 0.1"/>
  <!-- 好像不能传递方法 -->
  <BlogPost title="Blogging with vue" @enlarge-text="enlargeText()"/>
  <BlogPost title="Why vue is so fun" />
 </div>
 <h2>使用v-for传递props</h2>
 <div :style="{ fontSize: postFontSize + 'em' }">
  <BlogPost v-for="post in posts" :key="post.id" :title="post.title"
  @enlarge-text="postFontSize += 0.1"/>
 </div>
</template>