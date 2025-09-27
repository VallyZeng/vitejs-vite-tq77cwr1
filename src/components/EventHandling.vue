<script setup>
import { ref, warn } from 'vue'

const count = ref(0)
const name = ref('vue.js')

function green(event) {
 alert(`Hello ${name.value}!`)

 if(event) {
  alert(event.target.tagName)
 }
}

function say(message) {
 alert(message)
}
</script>

<template>
<div>
 <h3>内联事件处理器</h3>
 <button @click="count++">add 1</button>
 <p>count is : {{ count }}</p>
 <h3>方法事件处理器</h3>
 <button @click="green">Greet</button>
 <h3>在内联处理器中调用方法</h3>
 <button @click="say('Hello')">say hello</button>
 <button @click="say('bye')">say bye</button>
 <h3>在内联事件处理器中访问事件参数</h3>
 <h4>使用特殊的$event变量</h4>
 <button @click="warn('Form cannot to submitted yes.', $event)">
 Submit
 </button>
 <h4>使用箭头</h4>
 <button @click="(event) => warn('Form cannot to submitted yes.', event)">
 submitted
 </button>
</div>
</template>