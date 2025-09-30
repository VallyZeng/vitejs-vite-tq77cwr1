<script setup>

defineProps(['title', 'id'])
// 将事件暴露给父组件
defineEmits(['enlarge-text'])

</script>

<template>
<div class="blog-post">
 <h4>{{ id }} ,{{ title }}</h4>
 <!-- 将enlarge-text事件传递给父组件 -->
 <button @click="$emit('enlarge-text')">enlarge text</button>
</div>
</template>