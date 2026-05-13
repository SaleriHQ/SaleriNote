---
created: 2026-05-13
updated: 2026-05-13
tags:
 - 
---
## Vue3

### 声明式渲染

Vue 的核心是声明式渲染，通过拓展 html 的模板语法，我们可以根据 JavaScript 的状态描述 html 应该是什么样子，html 会根据 js 代码改变。

会因为值改变更新 html 页面的状态被称作响应式的， 如 `reactive()` 可以创建一个响应式的状态，它创建的对象都是 JavaScript Proxy。 

```js
import { reactive } from 'vue';
const counter = reactive({
	count: 0
});
console.log(counter.count);
counter.count++;
```

`reactive()`  只适用于对象，而另一个 api `ref` 可以接受任意类型的值，`ref` 会返回一个包裹对象，通过 `.value` 暴露内部的值。

```js
import { ref } from 'vue';
const message = ref("Hello World!");
console.log(message.value);
message.value =  "Changed";
```

在 Vue 中， 在 `<script setup>` 内声明的代码可以直接在模板中使用。

```vue
<h1>{{message}}</h1>
<p>Count is: {{ counter.count }}</p>
```

在模板中使用 `message` 不需要调用  `.value`， vue会自动解包。
在花括号中可以使用的是任意 JavaScript 语句

```vue
<h1>{{ message.split('').reverse().join(',') }}</h1>
```

### Attribute 绑定

在 Vue 中，mustache 语法（{{}}) 只能用于文本插值，为了给 attribute 绑定一个动态的值，所以我们需要使用 `v-bind` 指令。

```vue
<div> v-bind:id="divId"></div>
```

由 `v-` 开始的是特殊的 attribute ，是 Vue 模板语法的一部分，和文本插值类似，指令的值是可以访问 JavaScript 状态的表达式。
`:id` 是指令的参数，这个示例代码是将 `divId` 与 div 的 `id` 属性保持同步。
由于 `v-bind` 使用非常频繁，所以有一个简写语法。

```vue
<div :id="divId"></div>
```

### 事件监听

和前面的 `v-bind` 类似，我们可以使用 `v-on` 或 `@` 来监听 DOM 事件。

```vue
<button v-on:click="fun">{{count}}</button>
<button @click="fun">{{count}}</button>
```

其中 `fun` 是写在 `<script setup>` 内声明的函数。

### 表单绑定

可以按照前面学习的内容使用 `v-bind` 和  `v-on` 进行表单的绑定。

```vue
<input :value="text" @input="onInput" />
```
 
```js
function onInput(e) {
	text.value = e.target.value
}
```

 或者使用更简单的方式，使用 `v-model`

```vue
<input v-model="test" />
```

 这样表单的值就会和 `test` 同步了

### 条件渲染

 Vue 中可以使用 `v-if` 来使用状态控制元素渲染

```vue
<h1 v-if="status">Content</h1>
```

这样这个 `h1` 只会在 `staus` 为真的时候显示，为 `false` 时会直接从 DOM 中移除。
我们也可以使用 `v-else` 和 `v-else-if` 来表示其他的条件分支。

```vue
<h1 v-if="status">Content1</h1>
<h1 v-else>Content2</h1>
```

### 列表渲染

Vue 提供了 `v-for` 指令来渲染一个基于数组的列表

```vue
<ul>
	<li v-for="todo in todos" :key="todo.id">{{todo.text}}</li>
</ul>
```

  这里的 `todo` 是一个局部变量，用于迭代数组上的元素，我们还给每一个 todo 设置了唯一的 `id` ，并将其绑定到 `key` 上，使得 Vue 能精准移动到每一个 `<li>` 。

### 计算属性

`computed()` 会创建一个计算属性 `ref()`， 它会根据其他响应式数据源来计算它的 `.value`

```js
import { ref, computed } from 'vue';
const hideComputed = ref(false);
const todos = ref([
	...
]);
const filteredTodos = computed(() => {
	return hideComputed.value ? todos.value.filter((t) => t.done) : todos.value;
})
```

### 生命周期

前面使用 Vue 处理了所有的 DOM 更新，全部依赖于响应式和声明式渲染。然而不可避免的有时候需要手动操作 DOM,这时候就需要使用**模板引用**，指向模板中 DOM 元素的 ref。

```vue
<p ref="pElemenmtRef">hello</p>
```

```js
const pElementRef = ref(null)
```

这里 `ref` 使用 null 进行初始化时因为 `script` 进行 setup 时，DOM元素还不存在，模板引用智能在组建挂载之后进行访问。
要在挂载之后执行代码，可以使用 `onMounted`。

```js
import { onMounted } from 'vue';
onMounted(() => {
	// ....
});
```

这种就被称之为**生命周期钩子**，它允许注册一个在组件特定生命周期调用和回调的函数，还有其他的生命周期函数



## 修改记录