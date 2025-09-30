<script setup>
 import {ref, watch, watchEffect, watchPostEffect, watchSyncEffect} from 'vue'

 // 测试数据
 const count = ref(0)
 const message = ref('初始消息')
 const domElement = ref(null)

 const logs = ref([])

 function addLog(type, message) {
  logs.value.push(`[${new Date().toLocaleTimeString()}] ${type}: ${message}`)
 }

 // 1. 默认时机
 watch(count, (newVal, oldVal) => {
  addLog('默认Watch', `count: ${oldVal}-> ${newVal}`)
  addLog('默认Watch', `DOM内容: "${domElement.value?.textContent}"`)
  addLog('默认Watch', '---')
 })

 // 2. 后置时机,可以用 watchPostEffect 替代
 watch(count, (newVal, oldVal) => {
  addLog('后置Watch', `Count: ${oldVal}->${newVal}`)
  addLog('后置Watch', `DOM内容： "${domElement.value?.textContent}"`)
  addLog('后置Watch', '---')
 }, {
  flush: 'post'
 })
 // 3.同步时机
 watch(count, (newVal, oldVal) => {
  addLog('同步Watch', `Count: ${oldVal}->${newVal}`)
  addLog('同步Watch', `DOM内容： "${domElement.value?.textContent}"`)
  addLog('同步Watch', '---')
 }, {
  flush: 'sync'
 })

 //4 后置时机的另外一种简写
 watchPostEffect(() => {
  // 会在vue更新后执行
  if(count.value > 0) {
    addLog('WatchPostEffect', `DOM已更新，当前count: ${count.value}`)
  }
 })

 // 同步时机的另外一种简写
 watchSyncEffect(() => {
  // 这个会同步执行
  if(count.value > 0) {
   addLog('WatchSyncEffect', `同步检测到count变化：${count.value}`)
  }
 })

 function batchUpdate() {
  addLog('操作', '开始批量更新')

  // 连续修改响应式数据
  count.value++
  count.value++
  count.value++
  count.value++
  count.value++

  addLog('操作', '批量更新完成')
 }

 // 测试单个更新
 function singleUpdate() {
  count.value++
 }

 function reset() {
  count.value = 0
  logs.value = []
  addLog('系统', '已重置')
 }
</script>

<template>
 <h2>侦听器回调触发时机演示</h2>

 <div style="margin: 20px 0;">
  <button @click="singleUpdate">单个增加（+1）</button>
  <button @click="batchUpdate">批量增加（+5）</button>
  <button @click="reset" style="margin-left: 20px;">重置</button>
 </div>

 <div>
  <p>当前 count: <strong ref="domElement">{{ count }}</strong></p>
  <p>当前 message: {{ message }}</p>
 </div>

 <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 30px;">
  <div>
   <h3>执行时机说明</h3>
   <div style="text-align: lift; padding: 15px; background: #f5f5f5; border-radius: 8px;">
    <p><strong>默认（pre）</strong></p>
    <ul>
     <li>在组件更新前执行</li>
     <li>DOM 处于旧状态</li>
     <li>会进行批处理</li>
    </ul>
   </div>
  </div>

  <div>
   <h3>执行日志</h3>
   <div style="text-align: lift; padding: 15px; background: #f5f5f5; border-radius: 8px;">
    <div v-for="(log, index) in logs" :key="index" :style="{
     color: log.includes('默认watch') ? 'blue' : 'black',
     marginButton: '5px',
     fontSize: '14px'
    }">
    {{ log }} 
   </div>
   </div>
  </div>
 </div>
</template>