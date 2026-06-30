---
title: "哪位佬有成熟的vibecoding经验分享"
source: "https://linux.do/t/topic/2413287"
author:
  - "[[zlin]]"
published: 2026-06-16
created: 2026-06-21
description: "如题，虽然现在codex、claude code这种agent工具已经很厉害了（这两个都用了）。但是对我这种开发小白来说，小功能还可以，要实现一个功能复杂的项目还是非常困难的。 目前遇到几个问题： 1.写完初步需求和架构文档后，随不断开发，文档也在不断修改补充。但是随着上下文增多"
tags:
  - "clippings"
---
如题，虽然现在codex、claude code这种agent工具已经很厉害了（这两个都用了）。但是对我这种开发小白来说，小功能还可以，要实现一个功能复杂的项目还是非常困难的。

目前遇到几个问题：  
1.写完初步需求和架构文档后，随不断开发，文档也在不断修改补充。但是随着上下文增多，开发方向开始逐渐偏离主线，最后一堆屎山代码。  
2.前后端分离的系统。先写好前端，然后分模块对应开发接后端功能。还是开始先把后端数据调通，再接前端的页面，那种方式比较好？  
3.对于有些部分开发完测试需要可视化的看结果才能评估效果，这种情况下如何处理呢？（容易越搞越乱）  
4.前端页面达不到自己想要的效果，有时候和后端一对接，一修改就成一坨了。

楼主不算完全小白，有一些计算机知识，尝试了一些superpower类似的方案，但感觉没什么变化，各位佬平时是如何做到呢？求成熟经验分享，或者可以跟谁学一学，做几个vibecoding项目，十分感谢。

---

## Comments

> **GJKen** · [2026-06-16](https://linux.do/t/topic/2413287/2?u=salerihq)
> 
> 对于上下文太长:  
> 我一般让ai分析整个项目, 生成一份开发声明.md, 让后续新对话能够快速了解项目, 任务中途想中断或者上下文堆积够多了, 让ai写一份修改笔记.md, 避免上下文过长导致token消耗爆炸

> **Haleclipse** · [2026-06-16](https://linux.do/t/topic/2413287/3?u=salerihq)
> 
> 首先是 Coding 本身
> 
> 其次才是 歪脖抠腚！ 这只是一种形式

> **L.S** · [2026-06-16](https://linux.do/t/topic/2413287/4?u=salerihq)
> 
> [github.com/zts212653/clowder-ai](https://github.com/zts212653/clowder-ai/blob/main/cat-cafe-skills/feat-lifecycle/SKILL.md)
> 
> #### [cat-cafe-skills/feat-lifecycle/SKILL.md](https://github.com/zts212653/clowder-ai/blob/main/cat-cafe-skills/feat-lifecycle/SKILL.md)
> 
> [`main`](https://github.com/zts212653/clowder-ai/blob/main/cat-cafe-skills/feat-lifecycle/SKILL.md)
> 
> ```markdown
> ---
> name: feat-lifecycle
> description: >
>   Feature 立项、讨论、完成的全生命周期管理。
>   Use when: 开个新功能、new feature、F0xx、立项、feature 完成、验收通过、讨论新功能需求。
>   Not for: 代码实现、review、merge（那些有专门的 skill）。
>   Output: Feature 聚合文件 + BACKLOG 索引 + 真相源同步。
> triggers:
>   - "开个新功能"
>   - "new feature"
>   - "F0xx"
>   - "立项"
>   - "feature 完成"
>   - "F0xx done"
>   - "验收通过"
>   - "讨论新功能需求"
> argument-hint: "[阶段: kickoff|discussion|completion] [F0xx 或主题]"
> ---
> 
> # Feature Lifecycle
> ```
> 此文件已被截断。 [显示原始文件](https://github.com/zts212653/clowder-ai/blob/main/cat-cafe-skills/feat-lifecycle/SKILL.md)
> 
> 我自己是通过一阵套skills 其实是类似于 sdd的skills 来解决的，包括agent之间如何沟通交接如果验收 如何design gate让问题提前暴露 等等等  
> 
> [![image](https://cdn3.ldstatic.com/optimized/4X/1/a/c/1acdf82f76c61f0ce9a6a92960cc99bdfddabda1_2_690x375.jpeg)
> 
> image1920×1045 260 KB
> 
> ](https://cdn3.ldstatic.com/original/4X/1/a/c/1acdf82f76c61f0ce9a6a92960cc99bdfddabda1.jpeg "image")
> 
> 现在他们都会 和我先完成对齐 等对齐清楚之后 就各自合作完成coding review 最后alpha test 去点点点验收 以及最终回顾铲屎官的愿景 如果还有没对齐的继续loop