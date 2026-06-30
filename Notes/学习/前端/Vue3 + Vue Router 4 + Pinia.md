---
created: 2026-05-13
updated: 2026-05-13
tags:
  - 前端
  - 复习
  - Vue
  - VueRouter
  - Pinia
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
<div :id="divId"></div>
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

表单绑定还有几个修饰符
#### `.lazy` 
默认情况下，`v-model`会在`input`之后进行数据的更新，使用`.lazy`可以让它在每次`change`
之后进行数据的更新
#### `.number`
`.number`可以直接将用户的输入直接转化为数字
#### `.trim`
去除用户输入内容两端的空格

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

除此之外还有一个 `v-show`， 使用方式和 `v-if` 差别不大，但是 `v-show` 与 `v-if` 的区别是，`v-show` 隐藏元素是 `display:none`， 而 `v-if` 是直接从DOM移除

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

这里 `ref` 使用 null 进行初始化时因为 `script` 进行 setup 时，DOM元素还不存在，模板引用只能在组建挂载之后进行访问。
要在挂载之后执行代码，可以使用 `onMounted`。

```js
import { onMounted } from 'vue';
onMounted(() => {
	// ....
});
```

这种就被称之为**生命周期钩子**，它允许注册一个在组件特定生命周期调用和回调的函数，还有其他的[[生命周期]]函数
### 侦听器

有时候需要响应式的执行副作用，如一个状态被更新时将其输出到控制台，我们可以使用侦听器来实现。

```js
import { ref, watch } from 'vue';
const count = ref(0);
watch(count, (newCount) => {
	console.log(`new count is: ${newCount}`);
})
```

`watch()` 的作用是监听一个 `ref` ，当 `value` 改变就会触发回调。
### 组件

Vue 通常都是嵌套组件实现页面，可以引入别 的 `.vue` 组件使用。

```vue
<script setup>
import ChildComp from "./ChildComp.vue";
</script>
<template>
	<ChildComp />
</template>
```
### Props

子组件可以从父组件接受动态数据，首先，需要声明需要的 props。

```js
const props = defineProps({
	msg: String
})
```

`defineProps()`  是一个编译时宏，不需要导入，父组件可以像使用 attribute 一样传递 props。

```vue
<ChildComp :msg="greeting" />
```
### Emits

除了前面的数据传递，子组件还可以向父组件发出事件

```js
const emit = defineEmits(['response'])

emit('response', 'hello from child')
```

`emit` 的第一个参数是事件的名称，后续所有参数都传递给事件监听器

```vue
<ChildComp @response="(msg) => childMsg = msg" />
```

### 插槽

除了 props 外，父组件还可以使用**插槽(slots)**将模板片段传递给子组件。

```vue
<ChildComp>
	<h1>123123</h1>
</ChildComp>
```

在子组件中使用 `<slot>` 进行占位，作为之后插槽内容的出口。`<slot>` 内的内容作为未传递内容时的默认渲染选项。

## Vue Router 4

### 入门

Vue Router 是 Vue 的客户端路由解决方案，可以使用 Vue Router 通过 URL 决定要显示哪些组件。

#### 示例

```vue
<h1>Hello World</h1>
<p>Now Router is: {{ $route.fullPath }}</p>
<nav>
	<RouterLink to="/">home</RouterLink>
	<RouterLink to="/about">About</RouterLink>
</nav>
<main>
	<RouterView />
</main>
```

这是一个简单的 Vue Router 示例，使用了两个由 Vue Router提供的组件：`RouterLink`和`RouterView`。
在 Vue Router 中，我们不使用 `<a>` 创建连接，我们使用 Vue Router 提供的`RouterLink`，它能够使 Vue Router 在不重新加载页面的情况下改变 URL。
`RouterView`则是一个告诉 Vue Router 在哪里渲染当前 URL 对应组件的组件。
上面的示例代码还使用了 `$route.fullPath` 这是一个 Vue Router 注册的全局变量，用于访问一些关于 Router 的内容。
#### 路由实例创建

```js
import { createRouter, createMemoryHistory } from "vue-router";
import Home from "./pages/Home.vue";
import About from "./pages/About.vue";
const routes = [
	{
		path: "/",
		component: Home,
	},
	{
		path: "/about",
		component: About,
	},
];
const router = createRouter({
	history: createMemoryHistory(),
	routes,
});
```

路由实例通过 `createRouter` 进行创建，上述代码创建了一个 `/` 和 `/about` 的路由，分别对应了对应的组件，这些组件就是会被渲染到`RouterView`上的组件。
#### 注册

可以使用 `.use` 挂载到 Vue 上，然后就可以在各个页面进行使用。

```js
createApp(app).use(router).mount('#app');
```

### 动态路由匹配

#### 带参数的路由匹配

很多时候，需要用一个组件匹配很多个页面内容，我们可以使用一个动态字段来实现，称之为**路径参数**

```js
import User from './User.vue';
const routes = [
	{ path: '/users/:id', component: User },
]
```

现在 `/user/1` 和 `/user/2` 这样的 URL 都是同一个路由。
路径参数使用 `：` 表示，当一个路由匹配时，这个变量会被暴露到 `$route.params`。
或者我们可以使用多个参数

| 匹配模式                           | 匹配路径                      | route.params                           |
| ------------------------------ | ------------------------- | -------------------------------------- |
| /users/:username               | /users/SaleriHQ           | { username: 'SaleriHQ' }               |
| /users/:username/posts/:postId | /users/SaleriHQ/posts/123 | { username: 'SaleriHQ', postId: '123'} |
#### 响应式参数

使用带参数的路由时，从 `/users/1` 导航到 `/users/2` 时，相同的组件被复用，比起销毁再创建，复用显得更加高效。如果需要复用，可以简单的监听 `route.params`

```js
const route = useRoute();
watch(() => route.params.id, (newId, oldId) => {
	// 处理
});
```

或者使用 `beforeRouteUpdate` [[导航守卫]]，它可以直接取消导航。

```js
onBeforeRouteUpdate(async (to, from) => {
	// 处理
	userData.value = await fetchUser(to.params.id)
})
```

#### 捕获所有路由或者 404 路由

常规参数只匹配 URL 片段之间的内容，如果想要匹配任意路径，可以使用自定义的路径参数正则表达式，在路径参数之后的括号中加入正则表达式就行。

```js
const routes = [
	// 匹配所有内容并放置在 route.params.pathMath 下
	{ path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound},\
	// 匹配 /user- 开头的所有内容，并放置在 route.params.afterUser下
	{ path: '/user-:afterUser(.*)', component: UserGeneric }
]
```

### 嵌套路由

 一些应用的 UI 由多个层级构成，这样可以使用 URL 对应页面的嵌套层级。之前通过 `RouterView` 渲染了路由组件，当然也可以在被渲染的组件内再次使用 `RouterView` 渲染子路由。

```js
const routes = [
	{
		path: '/user/:id',
		component: User,
		children: [
			{
				path: 'profile',
				component: UserProfile
			},
			{
				path: 'posts',
				component: UserPosts
			}
		]
	}
]
```
 
如果希望匹配目前每匹配的剩余内容，可以使用一个空的嵌套路径进行匹配。

```js
const routes = [
	{
		path: '/user/:id',
		component: User,
		children: [
			{ path: '', component: UserHome }
		]
	}
]
```

### 编程式导航

除了使用`RouterLink`进行导航，还可以使用编写代码的方式进行导航。

```js
router.push('/users/Saleri')
router.push({ path: '/users/Saleri' })
router.push({ name: 'user', params: { username:'Saleri' }})
router.push({ path: '/register', query: { plan: 'private' }})
router.push({ path: '/about', hash: '#team' })
```

 不过要注意的是当制定`path`时，会直接忽略`params`这一属性。
#### 历史

```js
// 往前一条记录
router.go(1)
// 返回一条记录
router.go(-1)
// 前进3条记录
router.go(3)
// 如果没有充足的记录，会静默失败
router.go(100)
```

#### 替代当前位置

类似于`router.push`，不同的是他不会向history添加记录

声明式
```vue
<RouterLink :to='...' replace></RouterLink>
```

编程式

```js
router.push({ path: '/home', replace: true })
router.replace({ path: '/home' })
```

### 命名视图

有的时候希望同级展示多个视图，而不是嵌套展示，如创建一个`layout`，需要有一个`sidebar`和`main`两个视图，这个时候命名视图的作用就出现了，可以设置名字拥有多个视图。如果而没有设置名字，默认为`default`。

```vue
<RouterView class="view left-sidebar" name="LeftSidebar" />
<RouterView class="view main-content" />
<RouterView class="view right-sidebar" name="RightSidebar" />
```

```js
const routes = [
	{
		path: '/',
		components: {
			default: Home,
			LeftSidebar,
			RightSideBar
		}
	}
]
```
#### 嵌套命名视图


```vue
<div>
  <h1>User Settings</h1>
  <NavBar />
  <router-view />
  <router-view name="helper" />
</div>
```

```js
{
  path: '/settings',
  // 你也可以在顶级路由就配置命名视图
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```
### 重定向

```js
const routes = [{ path: '/home', redirect: '/' }]
```
### 导航守卫

#### 全局前置守卫

可以使用 `router.beforeEach` 注册一个全局前置守卫

```js
router.beforeEach((to, from) => {
	// 返回false取消导航
	return false
})
```

当一个导航处罚时，全局前置守卫按照创建顺序进行执行，守卫是异步执行，此时导航在所有守卫resolve前一直处于等待状态。
可以返回的值有：
- `false` ：取消当前导航
- `路由地址`：重定向到一个不同的地址

```js
router.beforeEach(async (to.from) => {
	if(!isAuthenticated && to.name !== 'login') {
		return { name: 'login' }
	}
})
```
## Pinia

Pinia是一个Vue的状态管理库，允许跨组件和页面进行状态共享。

```js
const pinia = createPinia();
createApp(App).use(pinia).use(router).mount("#app");
```

### 定义Store

Store由 `defineStore()` 定义，第一个参数要求独一无二的名字。

```js
export const useAlertsStore = defineStore('alerts', {
	// config
})
```
#### Option Store

我们在`defineStore`内定义`Store`的`state`（数据），`getters`（计算属性）,`actions`（方法）。

```js
export const useCounterStore = defineStore('counter', {
	state: () => ({ count: 0, name: 'Saleri' }),
	getters: {
		doubleCount: (state) => state.count * 2,
	},
	actions: {
		increment() {
			this.count++
		}
	}
})
```
#### Setup Store

使用另一种语法格式，类似于 Vue 组合API

```js
export const useCounterStore = defineStore('counter', () => {
	const count = ref(0)
	const name = ref('Saleri),
	const doubleCount = computed(() => count.value * 2)
	function increment() {
		count.value++
	}
	return { count, name, doubleCount, increment }
})
```

> 需要注意的是虽然 `Setup Store` 比 `Option Store` 更加的自由，但是相对的，使用 `Setup Store` 也会有限制，如在结尾必须返回`state`所有属性，不能有私有属性，不完整返回会影响SSR,开发工具和其他插件的运行。

Setup store也可也访问依赖于全局提供的属性，比如路由，任何应用层面提供的属性都可也在 store 中使用 `inject()` 访问，就像在组件中一样。

```js
export const useSearchFilters = defineStore('search-filters', () => {
	const route = useRoute()
	// 这里假定 `app.provide('appProvided', 'value')` 已经调用过
	const appProvided = inject('appProvided')
	// ...
	return {
		// ...
	}
})
```

> [!WARNING]  
> 不要返回如`route`或`appProvided`之类的属性，因为它们不属于store。
### 使用 Store

前面的代码只是定义了 store, 在调用之前是不会被创建的。

```js
const store = useCounterStore()
```

> [!TIP]
> 我们可以定义任意多的 store, 但是为例让使用 pinia 的益处最大化（如构建工具自动进行代码分割及Typescript推断），我们应该在不同文件进行 store 的定义

不过要注意的是 store 是一个 `reactive` 包装的对象，这意味着不需要再 getters 后面写 `.value`，同时也不能对他们进行结构。

```js hl:2-4
const store = useCounterStore();

const { name, doubleCount } = store;// ❌错误，破坏了响应式
name // ❌错误，会一直是Saleri不会响应式更新
doubleCount // ❌错误，会一直是0

setTimeout(() => {
	store.increment()
}, 1000)
const doubleValue = computed(() => store.doubleValue)
```
### State

多数情况下，State都是 store 的核心，在 pinia 中，state被定义为一个会返回初始状态的
函数，使得pinia支持服务端和客户端。
#### 重置 state

使用选项式 API 可以直接使用 `$reset()`  进行重置。

```js
const store = useStore()
store.$reset()
```

如果使用的是组合式 API, 则需要自行定义。

```js
export const useCounterStore = defineStore('counter', () => {
	const count = ref(0)
	function $reset() {
		count.value = 0
	}
	return { count, $reset }
})
```

## 修改记录

- 2026.5.13@12:35 完成 Vue 的复习
- 2026.5.13@17.49 完成 Vue Router 的学习
- 2026.5.13@23.12 完成 Pinia 的学习