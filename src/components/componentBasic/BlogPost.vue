<script setup>
// 一个编译宏命令，声明的props会自动暴露给模板，只有在 script setup中才可用
defineProps(['title'])
// 声明本组件可能触发的所有事件
defineEmits(['enlarge-text'])
</script>

<template>
 <h4>{{ title }}</h4>
 <button @click="$emit('enlarge-text')">Enlarge text</button>
</template>