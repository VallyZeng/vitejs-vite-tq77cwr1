<script setup>
import { ref } from 'vue'

const message = ref('');
const checked = ref(true);
const checkedNames = ref([])
const picked = ref([])
const selected = ref('')
const selectedArr = ref([])
const options = ref([
 { text: 'One', value: 'A' },
 { text: 'Two', value: 'B' },
 { text: 'Three', value: 'C' }
])
const dynamicTrueValue = ref('true')
const dynamicFalseValue = ref('false')
const toggle = ref('')
const pick = ref('')
const msg = ref('')
const age = ref()

</script>

<template>
<h2>基本用法</h2>
<h3>表单输入绑定</h3>
<p>Message is {{ message }}</p>
<input v-model="message" placeholder="edit me" />
<h3>多行文本</h3>
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
<h3>复选框</h3>
<input type="checkbox" id="checkbox" v-model="checked"/>
<label for="checkbox">{{ checked }}</label>
<h3>多个复选框绑定</h3>
<div>checked names: {{ checkedNames }}</div>
<input type="checkbox" id="jack" value="jack" v-model="checkedNames"/>
<label for="jack">jack</label>
<input type="checkbox" id="john" value="john" v-model="checkedNames" />
<label for="john">john</label>
<input type="checkbox" id="mike" value="mike" v-model="checkedNames" />
<label for="mike">mike</label>
<h3>单选按钮</h3>
<div>Picked: {{picked}}</div>
<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>
<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
<h3>选择器</h3>
<div>selected: {{ selected }}</div>

<select v-model="selected">
 <option disabled value="">Please select one</option>
 <option>A</option>
 <option>B</option>
 <option>C</option>
</select>
<p>多选</p>
<div>selectedArr: {{ selectedArr }}</div>
<select v-model="selectedArr" multiple>
 <option disabled value="">Please select one</option>
 <option>A</option>
 <option>B</option>
 <option>C</option>
</select>
<p>动态渲染选择器</p>
<select v-model="selectedArr" multiple>
 <option disabled value="">Please select one</option>
 <option v-for="option in options" :value="option.value">{{ option.text }}</option>
</select>
<h2>值绑定</h2>
这样的话，v-model就可以绑定到动态值了
<input type="radio" id="a" v-model="picked" value="a"/>
<label for="a">A</label>
<input type="checkbox" v-model="toggle" />
<select v-model="selected">
 <option value="abc">ABC</option>
</select>
<h3>复选框</h3>
选择之后，toggle会被修改成true或者false
<input type="checkbox" id="bind-checkbox" v-model="toggle" true-value="true" false-value="false" />
<label for="bind-checkbox">复选</label>
<p>也可以修改成动态字段</p>
<input type="checkbox" id="dynamic-bind-checkbox" v-model="toggle" true-value="dynamicTrueValue" false-value="dynamicFalseValue" />
<label for="dynamic-bind-checkbox">复选</label>
<div>{{ toggle }}</div>
<h3>单选按钮</h3>
<input type="radio" id="bind-pick" v-model="pick" :value="first" />
<input type="radio" id="bind-pick-second" v-model="pick" :value="second" />
<div>{{ pick }}</div>
<h3>选择器选项</h3>
<select v-model="selected">
<!-- 内联对象字面值 -->
 <option :value="{ number: 123 }">123</option>
</select>
<div>{{ selected }}</div>
<h2>修饰符</h2>
<h3>.lazy</h3>
<!-- 输入后，回车（触发change事件）才会更新 -->
<input v-model.lazy="msg" />
<div>{{ msg }}</div>
<h3>.number</h3>
<!-- 会自动转换为数字，如果无法转换，则返回原始值 -->
<input v-model.number="age" />
<div>{{ age }}</div>
<h3>.trim</h3>
<!-- 去除掉字符串左右两边空格 -->
<input v-model.trim="age" />
</template>