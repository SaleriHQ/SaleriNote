---
created: 2026-05-14
updated: 2026-05-14
tags:
 - 
---
## Typescript 基础

Typescript 是 Javascript 的一个超集，弥补了 JS 没有了类型检测的缺点。TypeScript 由 Microsoft 开发维护，于 2012 年首次发布，经过多年的发展已成为前端领域最主流的编程语言之一。其核心设计理念是在 JavaScript 的基础上添加**静态类型系统**和**面向对象特性**，让开发者能够在编译阶段提前发现类型错误，提升代码质量和可维护性。

TypeScript 最终会被编译成纯 JavaScript 代码，可以在任何支持 ES3+ 的浏览器、Node.js 或其他 JavaScript 运行时环境中运行。这意味着我们可以渐进式地在现有 JS 项目中引入 TS，逐步享受类型检查带来的好处，而无需完全重写代码库。
### 环境搭建

如果使用 Vscode 进行代码开发，首先需要对软件进行设置。
- Function Like Return Types，显示推导得到的函数返回值类型
- Parameter Names，显示函数入参的名称
- Parameter Types，显示函数入参的类型
- Variable Types，显示变量的类型。
### 数据类型

 #### 基础数据类型

```ts
const userNick = 'SaleriHQ' // string
const userAge = 18 // number
const userMarried = false // boolean
const userNull = null // null
const userUndefined = undefined // undefined
const userObject = {} // object
const userArray = [] // array
```
#### 接口

```ts
interface User {
	userName: string;
	userAge:  number;
	userMarried: boolean;
}
const user: User = {
	userName: 'test',
	userAge: 29,
	userMarried: false,
}
```

同时接口也可也是别的接口的内部元素的类型，也可以作为数组使用

```ts
interface Job {...}
interface User{
	userJob: Job;
}
const userList: User[] = ...;
```

像前面这样定义接口会让内部的元素变为必须，创建的实例必须和接口的结构一模一样，除非使用`?:`。

```ts
interface User {
	userJob?: Job;
}
```

#### Enum

使用Enum定义常量可以避免魔法数字没有注释定义的问题

```ts
enum UserLevelCode {
	Visitor = 10001,
	NonVIPUser = 10002,
	VIPUser = 10003,
	Admin = 10010,
}
```

同时枚举同时支持数字，字符串，函数计算

```ts
enum UserLevelCode {
	Visitor = 10001,
	Mixed = 'Mixed',
	Random = generate(),
}
```
### 函数

函数可以用以下方式声明参数类型和返回值类型。

```ts
const handler = (args: string[]): number => {
}
function handler(args: string[]): number {
}
```
#### 类型别名

我们可以为类型创建不同的别名。

```ts
type MagicString = string;
type Sum = (a: number, b: number) => number;
const sum: Sum = function(a, b) {
}
```
#### 函数重载

在 JS 中，重载函数非常的麻烦，需要在函数内做类型判断然后处理，但是使用 TS 就可以简单的进行函数重载。

```ts
function sum(base: number, incre: number): number;
function sum(baseArray: number[], incre: number): number[]; 
function sum(incre: number, baseArray: number[]): number[]; 
function sum(baseArray: number[], increArray: number[]): number[]; 
function sum(x: number | number[], y: number | number[]): number | number[] { }
```
### 类型编程

上述只是进行了简单的类型注释，接下来展示 TS 更为复杂的一部分。
#### any & unknown 与类型断言

在实际开发中，有时无法确定变量的类型或需要处理动态数据，`any` 和 `unknown` 就是为此设计的。

```ts
// any：绕过所有类型检查，不推荐滥用
let dynamicValue: any = 'hello';
dynamicValue = 123;
dynamicValue = { name: 'test' };
dynamicValue(); // 不会报错，运行时可能出错

// unknown：类型安全版本的 any
let uncertainValue: unknown = 'hello';
uncertainValue = 123;
// uncertainValue(); // 报错，必须先缩小类型范围

// 类型断言：手动指定变量类型
function getLength(value: unknown): number {
    if (typeof value === 'string') {
        return value.length; // TS 自动推断为 string
    }
    if (Array.isArray(value)) {
        return value.length;
    }
    return 0;
}

// as 语法（推荐）
const name: unknown = 'Saleri';
const nameStr = name as string;

// 非空断言：告诉 TS 变量一定不为 null/undefined
function processData(data?: string) {
    console.log(data!.toUpperCase()); // 确保 data 有值
}
```

**最佳实践**：尽量避免使用 `any`，优先使用 `unknown`。必须使用 any 时，建议添加注释说明原因。

#### 类型别名

使用 `type` 关键字为复杂类型定义别名，提高代码可读性和复用性。

```ts
// 基本类型别名
type ID = string | number;
type Callback = () => void;

// 接口类型别名
type User = {
    name: string;
    age: number;
};

// 函数类型别名
type AddFn = (a: number, b: number) => number;
const add: AddFn = (x, y) => x + y;

// 类型别名 vs 接口的区别
// - interface 可以被合并（声明合并），type 不行
// - interface 用于定义对象结构，type 更灵活
// - type 可以定义联合类型、交叉类型、元组

interface Animal {
    name: string;
}
interface Animal {
    age: number;
} // 自动合并，等价于 { name: string; age: number }

// 推荐：简单对象用 interface，复杂联合类型用 type
```

#### 联合类型与交叉类型

联合类型表示"或"，交叉类型表示"且"。

```ts
// 联合类型：值可以是多种类型之一
type Status = 'pending' | 'success' | 'error';
type Result = string | number | null;

function getResult(status: Status): string {
    if (status === 'pending') return '加载中...';
    if (status === 'success') return '成功';
    return '失败';
}

// 交叉类型：合并多个类型的属性
type Nameable = { name: string };
type Ageable = { age: number };
type Person = Nameable & Ageable; // 等价于 { name: string; age: number }

// 实际应用：合并配置对象
type BaseConfig = { debug: boolean };
type APIConfig = { baseURL: string; timeout: number };
type AppConfig = BaseConfig & APIConfig;

const config: AppConfig = {
    debug: true,
    baseURL: 'https://api.example.com',
    timeout: 5000,
};
```

#### 泛型

泛型允许创建可复用的组件，同时保持类型安全。

```ts
// 泛型函数
function identity<T>(value: T): T {
    return value;
}
identity<string>('hello');
identity(123); // TS 自动推断为 number

// 泛型约束：限制类型范围
interface HasLength {
    length: number;
}
function getLength<T extends HasLength>(value: T): number {
    return value.length;
}
getLength('hello');    // OK
getLength([1, 2, 3]); // OK
// getLength(123);     // Error: number 没有 length 属性

// 泛型接口
interface Pair<K, V> {
    key: K;
    value: V;
}
const userPair: Pair<string, number> = { key: 'age', value: 25 };

// 泛型默认值
interface Response<T = string> {
    code: number;
    data: T;
}
const resp: Response = { code: 200, data: 'success' };
```

#### 类型声明与类型合并

TypeScript 允许同名接口自动合并，声明文件用于描述已有 JavaScript 库的类型。

```ts
// 声明合并：同名接口自动合并属性
interface Logger {
    log(msg: string): void;
}
interface Logger {
    error(msg: string): void;
}
const logger: Logger = {
    log: (msg) => console.log(msg),
    error: (msg) => console.error(msg),
};

// .d.ts 类型声明文件示例
// global.d.ts
declare global {
    interface Window {
        myGlobalFn: () => void;
    }
}

// 模块声明
declare module 'my-lib' {
    export function doSomething(): void;
}

// 使用 namespace 组织类型
declare namespace MyModule {
    function helper(): void;
    const VERSION: string;
}
```

#### 内联工具类型

TypeScript 内置了许多实用工具类型，用于类型转换。

```ts
// Partial<T>：所有属性变为可选
interface User {
    name: string;
    age: number;
}
type PartialUser = Partial<User>;
// 等价于 { name?: string; age?: number; }

// Required<T>：所有属性变为必需
type RequiredUser = Required<PartialUser>; // 还原为 User

// Pick<T, K>：从 T 中选取指定属性
type UserName = Pick<User, 'name'>; // { name: string }

// Omit<T, K>：从 T 中排除指定属性
type UserAge = Omit<User, 'name'>; // { age: number }

// Record<K, V>：构建键值对类型
type Status = 'success' | 'error' | 'loading';
type StatusInfo = Record<Status, { code: number; msg: string }>;
const statusMap: StatusInfo = {
    success: { code: 200, msg: '成功' },
    error: { code: 500, msg: '失败' },
    loading: { code: 0, msg: '加载中' },
};

// Exclude<T, U>：排除联合类型中的部分
type T0 = Exclude<'a' | 'b' | 'c', 'a'>; // 'b' | 'c'
type T1 = Exclude<string | number, string>; // number

// ReturnType<T>：获取函数返回值类型
function getUser() {
    return { name: 'Saleri', age: 18 };
}
type User = ReturnType<typeof getUser>; // { name: string; age: number }

// Parameters<T>：获取函数参数类型
function handleData(id: number, name: string) {}
type Params = Parameters<typeof handleData>; // [id: number, name: string]
```

#### 模板字符串类型

模板字符串类型允许创建基于字符串操作的类型。

```ts
// 基本用法
type EventName = 'click' | 'focus' | 'blur';
type HandlerName = `on${Capitalize<EventName>}`;
// HandlerName = 'onClick' | 'onFocus' | 'onBlur'

// 拼接字符串
type Prefix = 'get' | 'set' | 'delete';
type MethodName = `${Prefix}User`;
// MethodName = 'getUser' | 'setUser' | 'deleteUser'

// 类型模式匹配
type ExtractRoute<T extends string> = T extends `${infer Method} /api/${infer Path}` ? `${Method}:${Path}` : never;
type Route = ExtractRoute<'POST /api/users'>; // 'POST:users'

// 实际应用：CSS 单位
type Size = 'px' | 'em' | 'rem';
type CSSValue = `${number}${Size}`;
const width: CSSValue = '10px';
// const invalid: CSSValue = '10'; // Error
```

#### TypeScript 配置

`tsconfig.json` 是 TypeScript 项目的核心配置文件。

```json
{
  "compilerOptions": {
    // 目标 JavaScript 版本
    "target": "ES2020",
    "module": "ESNext",
    
    // 模块解析策略
    "moduleResolution": "node",
    
    // 输出目录
    "outDir": "./dist",
    
    // 源码目录
    "rootDir": "./src",
    
    // 严格模式（强烈建议开启）
    "strict": true,
    
    // 启用所有严格类型检查
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictPropertyInitialization": true,
    
    // 允许默认导入
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    
    // 跳过库检查
    "skipLibCheck": true,
    
    // 解析 JSON 模块
    "resolveJsonModule": true,
    
    // 生成 .d.ts 文件
    "declaration": true,
    
    // 生成 source map
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

**关键配置项说明**：
- `"strict": true`：启用所有严格检查，是生产环境的推荐配置
- `"noImplicitAny": true`：不允许隐式 any 类型
- `"strictNullChecks": true`：严格检查 null/undefined
- `"noUnusedLocals": true`：不允许未使用的局部变量
- `"skipLibCheck": true`：跳过第三方库的类型检查，加速编译

---

## Vue 3 + TypeScript 实战

### 组件定义

Vue 3 中推荐使用 `<script setup>` 语法，结合 TypeScript 实现类型安全的组件。

```ts
<script setup lang="ts">
import { ref, computed, defineProps, defineEmits } from 'vue';

// Props 定义（推荐使用类型声明）
interface Props {
    title: string;
    count?: number;
    items: string[];
}

const props = withDefaults(defineProps<Props>(), {
    count: 0,
});

// Emits 定义
interface Emits {
    (e: 'update', value: string): void;
    (e: 'delete', id: number): void;
}
const emit = defineEmits<Emits>();

// 响应式数据
const message = ref<string>('Hello');
const userInfo = ref<{ name: string; age: number } | null>(null);

// 计算属性
const totalCount = computed<number>(() => {
    return props.count + props.items.length;
});

// 方法
function handleClick() {
    emit('update', message.value);
}

function handleDelete(id: number) {
    emit('delete', id);
}
</script>
```

### 响应式类型

Vue 3 的响应式系统与 TypeScript 配合使用时需要注意类型推导。

```ts
<script setup lang="ts">
import { ref, reactive, computed, toRefs } from 'vue';

// ref：适合基本类型，会自动解包
const name = ref<string>('Saleri');
const age = ref<number>(18);

// ref 需要 .value 访问，但模板中自动解包
name.value = 'New Name';

// reactive：适合对象类型
interface User {
    id: number;
    profile: {
        name: string;
        avatar: string;
    };
    tags: string[];
}

const user = reactive<User>({
    id: 1,
    profile: {
        name: 'Saleri',
        avatar: 'https://example.com/avatar.png',
    },
    tags: ['developer', 'typescript'],
});

// toRefs：将 reactive 转为 ref，保持响应式
const { id, profile, tags } = toRefs(user);

// computed：自动类型推导
const userName = computed(() => user.profile.name);

// watchEffect / watch
import { watch, watchEffect } from 'vue';

watch(name, (newValue, oldValue) => {
    console.log(`Name changed: ${oldValue} -> ${newValue}`);
});

// 监听深度属性
watch(
    () => user.profile.name,
    (newName) => {
        console.log('Profile name updated:', newName);
    }
);
</script>
```

### 父子组件通信

Vue 3 提供了完整的类型安全的组件通信方案。

```ts
// 子组件：UserCard.vue
<script setup lang="ts">
// 使用泛型定义 Props
interface Props {
    user: {
        id: number;
        name: string;
        email?: string;
    };
    variant?: 'primary' | 'secondary';
}

const props = withDefaults(defineProps<Props>(), {
    variant: 'primary',
});

// 定义 Emits
const emit = defineEmits<{
    (e: 'select', userId: number): void;
    (e: 'update:user', user: Props['user']): void;
}>();

// v-model 支持
const { user } = toRefs(props);
</script>

<template>
    <div :class="variant">
        <h3>{{ user.name }}</h3>
        <p v-if="user.email">{{ user.email }}</p>
        <button @click="emit('select', user.id)">选择</button>
    </div>
</template>
```

```ts
// 父组件使用
<script setup lang="ts">
import UserCard from './UserCard.vue';

const selectedUser = ref<{ id: number; name: string; email?: string }>({
    id: 1,
    name: 'Saleri',
    email: 'saleri@example.com',
});

function handleSelect(id: number) {
    console.log('Selected:', id);
}

function handleUpdate(user: typeof selectedUser.value) {
    selectedUser.value = user;
}
</script>

<template>
    <UserCard
        v-model:user="selectedUser"
        variant="primary"
        @select="handleSelect"
    />
</template>
```

### 插槽与依赖注入

Provide / Inject 是跨层级组件通信的主要方式。

```ts
// 提供者：Parent.vue
<script setup lang="ts">
import { provide, ref } from 'vue';

interface ThemeContext {
    primaryColor: string;
    setColor: (color: string) => void;
}

const primaryColor = ref('#1890ff');

const themeContext = computed<ThemeContext>(() => ({
    primaryColor: primaryColor.value,
    setColor: (color: string) => {
        primaryColor.value = color;
    },
}));

provide<ThemeContext>('theme', themeContext);
</script>

<template>
    <div>
        <slot />
    </div>
</template>
```

```ts
// 消费者：Child.vue
<script setup lang="ts">
import { inject } from 'vue';

interface ThemeContext {
    primaryColor: string;
    setColor: (color: string) => void;
}

const theme = inject<ThemeContext>('theme');

function changeTheme() {
    theme?.setColor('#52c41a');
}
</script>

<template>
    <button :style="{ background: theme?.primaryColor }">
        Themed Button
    </button>
</template>
```

### 组合式函数（Composables）

组合式函数是 Vue 3 中组织逻辑的主要方式，TypeScript 让它变得类型安全。

```ts
// useUser.ts
import { ref, computed, type Ref, type ComputedRef } from 'vue';

interface User {
    id: number;
    name: string;
    role: 'admin' | 'user' | 'guest';
}

interface UseUserReturn {
    user: Ref<User | null>;
    isLoggedIn: ComputedRef<boolean>;
    isAdmin: ComputedRef<boolean>;
    login: (user: User) => void;
    logout: () => void;
}

export function useUser(): UseUserReturn {
    const user = ref<User | null>(null);

    const isLoggedIn = computed(() => user.value !== null);
    const isAdmin = computed(() => user.value?.role === 'admin');

    function login(userData: User) {
        user.value = userData;
    }

    function logout() {
        user.value = null;
    }

    return {
        user,
        isLoggedIn,
        isAdmin,
        login,
        logout,
    };
}
```

```ts
// 使用组合式函数
<script setup lang="ts">
import { useUser } from './composables/useUser';

const { user, isLoggedIn, isAdmin, login, logout } = useUser();

function handleLogin() {
    login({
        id: 1,
        name: 'Saleri',
        role: 'admin',
    });
}
</script>
```

### 路由类型

Vue Router 4 与 TypeScript 结合实现完整的路由类型安全。

```ts
// router/index.ts
import { createRouter, createWebHistory, type RouteRecordRaw } from 'vue-router';

const routes: RouteRecordRaw[] = [
    {
        path: '/',
        name: 'Home',
        component: () => import('../views/Home.vue'),
    },
    {
        path: '/user/:id',
        name: 'UserDetail',
        component: () => import('../views/UserDetail.vue'),
        props: true,
    },
];

const router = createRouter({
    history: createWebHistory(),
    routes,
});

export default router;
```

```ts
// 在组件中使用路由类型
<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router';
import { computed } from 'vue';

const route = useRoute();
const router = useRouter();

// 自动类型推导：route.params.id
const userId = computed(() => route.params.id as string);

// 编程式导航
function navigateToUser(id: number) {
    router.push({
        name: 'UserDetail',
        params: { id: String(id) },
    });
}
</script>
```

### Pinia 状态管理

Pinia 是 Vue 3 官方推荐的状态管理库，原生支持 TypeScript。

```ts
// stores/user.ts
import { defineStore } from 'pinia';

interface UserState {
    id: number | null;
    name: string;
    email: string | null;
    permissions: string[];
}

interface UserActions {
    login: (name: string, email: string) => void;
    logout: () => void;
    hasPermission: (permission: string) => boolean;
}

export const useUserStore = defineStore<'user', UserState, {}, UserActions>(
    'user',
    {
        state: (): UserState => ({
            id: null,
            name: '',
            email: null,
            permissions: [],
        }),
        getters: {
            isLoggedIn: (state) => state.id !== null,
        },
        actions: {
            login(name: string, email: string) {
                this.id = Date.now();
                this.name = name;
                this.email = email;
                this.permissions = ['read', 'write'];
            },
            logout() {
                this.$reset();
            },
            hasPermission(permission: string): boolean {
                return this.permissions.includes(permission);
            },
        },
    }
);
```

```ts
// 在组件中使用
<script setup lang="ts">
import { storeToRefs } from 'pinia';
import { useUserStore } from '../stores/user';

const userStore = useUserStore();
const { name, isLoggedIn } = storeToRefs(userStore);

function handleLogin() {
    userStore.login('Saleri', 'saleri@example.com');
}
</script>

<template>
    <div v-if="isLoggedIn">
        <p>Welcome, {{ name }}</p>
        <button @click="userStore.logout">Logout</button>
    </div>
</template>
```

---

## 修改记录