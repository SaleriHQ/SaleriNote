---
created: 2026-05-17
updated: 2026-05-17
tags:
 - 
---
## 关系

async/await 是 Promise 的语法糖

```js
// async/await
asycn function getData() {
	const res = await fetch('/api/data/);
	return res;
}
// Promsie
function getData() {
	return fetch('/api/data').then(res => res); 
}
```

async 函数始终返回的是 Promsie, await相当于`.then`的同步写法
## 写法区别

Promsie 是用 `.then` 完成链式调用，`async/await`是用`await`风格，链式调用时，then容易嵌套过深，使用`async/await`更加清晰，并行请求时 两者相同。
### 异常捕获

Promise 是用 `.catch` 进行异常捕获，支持 `.finally` 最终执行，`async/await` 是用 `try-catch-finally` 捕获错误，结构清晰，同步风格代码更加易读，`await` 抛出的  `reject` 会被 `catch` 捕获。。
### 适用场景

Promise 更适合单次回调，`async/await` 更适合复杂流程控制、多个顺序依赖的异步操作，调试也更加方便。