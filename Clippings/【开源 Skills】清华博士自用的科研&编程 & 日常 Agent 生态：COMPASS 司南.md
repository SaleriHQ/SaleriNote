---
title: "【开源 Skills】清华博士自用的科研&编程 & 日常 Agent 生态：COMPASS 司南"
source: "https://linux.do/t/topic/2416892/6"
author:
  - "[[sauterne]]"
published: 2026-06-16
created: 2026-06-21
description: "本帖使用社区开源推广，符合推广要求。我申明并遵循社区要求的以下内容： 我的帖子已经打上 开源推广 标签： 是 我的开源项目完整开源，无未开源部分： 是 我的开源项目已链接认可 LINUX DO 社区： 是 我帖子内的项目介绍，AI 生成、润色内容部分已截图发出： 是 以上选择我"
tags:
  - "clippings"
---
#### 本帖使用社区开源推广，符合推广要求。我申明并遵循社区要求的以下内容：

- **我的帖子已经打上 [开源推广](https://linux.do/tag/2234-tag/2234) 标签：** 是
- **我的开源项目完整开源，无未开源部分：** 是
- **我的开源项目已链接认可 LINUX DO 社区：** 是
- **我帖子内的项目介绍，AI 生成、润色内容部分已截图发出：** 是
- **以上选择我承诺是永久有效的，接受社区和佬友监督：** 是

为了高效的科研，尽量发论文毕业，我从 openai 刚出来就在使用各种 AI 模型。并且从今天年初使用 Openclaw 的时候就在研究迭代各种 skill。本着开源的态度，我把自己的这套 “自认为” 高效好用的 skill 分享出来整理成了一个开源 repo：

**COMPASS 司南：Personal Alignment skill OS for AI Agents**  
GitHub：[GitHub - dongshuyan/compass-skills: 司南：个性化 AI 任务总控 Skills 系统 /COMPASS: Personal Alignment Skills OS for AI Agents · GitHub](https://github.com/dongshuyan/compass-skills)  
如果你觉得这些 skills 有用，欢迎使用，Star，fork，以及提意见。

一键安装

```bash
npx skills add dongshuyan/compass-skills --skill '*' -a codex -a claude-code
```

## 它有什么用？

**场景 1：任务开始前，准确理解用户需求。**  
这个skill只确保3件事：  
1.帮助用户完全理解自己的需求  
2.帮助AI完全理解用户的需求  
3.让用户知道AI完全理解了他的需求

**场景 2：任务进行中和结束后，自动构建任务树\*任务森林。**  
自动把当前 session 的目标、进度、偏差、依赖、待办和决策写成 proposal。确认后生成树视图、DAG 视图、任务详情卡和推荐队列，让下一个 agent 或下一个 session 继续时知道 “这件事为什么做、做到哪、下一步做什么”。

**场景 3：长期协作中，让 AI 成为最懂你的人。**  
在本地保存可审计、可纠错、可撤回的用户画像来辅助其他skill给出更准确的回答。

## 生成结果展示：

[![image](https://cdn3.ldstatic.com/optimized/4X/d/1/1/d119eed0e85b65db8766f4d19b1bee9bd41a458e_2_690x389.png)

image1642×928 105 KB

](https://cdn3.ldstatic.com/original/4X/d/1/1/d119eed0e85b65db8766f4d19b1bee9bd41a458e.png "image")

根据用户的每个 session 自动生成任务关系树视图与 session 更新流程：

![task-forest tree demo](https://cdn3.ldstatic.com/original/4X/5/d/d/5dd848aead3473da06fb3d952023b7346e9e1c9b.webp)

这个 GIF 是多个 session 运行当前skill之后生成的html里面展示的当前repo逐步完善的动态过程

DAG 关系视图实况截图：

[![task-forest live DAG view](https://cdn3.ldstatic.com/optimized/4X/0/0/1/0015867f788bf13bf4d54f39daa717c3ab7f4fbe_2_690x366.jpeg)

task-forest live DAG view1920×1019 298 KB

](https://cdn3.ldstatic.com/original/4X/0/0/1/0015867f788bf13bf4d54f39daa717c3ab7f4fbe.jpeg "task-forest live DAG view")

自动生成的任务详情、目的、要求、证据和调度建议实况截图：

[![task-forest live detail view](https://cdn3.ldstatic.com/optimized/4X/b/6/0/b60cf351317237b3a03761298fe1bfe7d057cb23_2_690x366.jpeg)

task-forest live detail view1920×1021 276 KB

](https://cdn3.ldstatic.com/original/4X/b/6/0/b60cf351317237b3a03761298fe1bfe7d057cb23.jpeg "task-forest live detail view")

用户画像与需求对齐的协作方式：

[![司南用户画像与需求对齐流程](https://cdn3.ldstatic.com/optimized/4X/5/0/4/5045321a4e6ed04aecd2d62042575604df17fffb_2_690x460.jpeg)

司南用户画像与需求对齐流程1536×1024 333 KB

](https://cdn3.ldstatic.com/original/4X/5/0/4/5045321a4e6ed04aecd2d62042575604df17fffb.jpeg "司南用户画像与需求对齐流程")

[![image](https://cdn3.ldstatic.com/optimized/4X/9/e/a/9ea51e4e7cfcd1904d3b73cece66a91432209b7f_2_690x334.png)

image2804×1358 417 KB

](https://cdn3.ldstatic.com/original/4X/9/e/a/9ea51e4e7cfcd1904d3b73cece66a91432209b7f.png "image")

## **生态图**

[![image](https://cdn3.ldstatic.com/optimized/4X/f/8/d/f8d99df68b43f069d60baa7fb4525d86edfee3d6_2_690x414.png)

image1500×900 82.7 KB

](https://cdn3.ldstatic.com/original/4X/f/8/d/f8d99df68b43f069d60baa7fb4525d86edfee3d6.png "image")

[![image](https://cdn3.ldstatic.com/optimized/4X/9/e/7/9e7a15389146f47fbc7694819101121073ccaffe_2_690x322.png)

image2822×1318 471 KB

](https://cdn3.ldstatic.com/original/4X/9/e/7/9e7a15389146f47fbc7694819101121073ccaffe.png "image")

  

[![image](https://cdn3.ldstatic.com/optimized/4X/9/1/7/917e0220aea31702561267ccbbf30e6ef0ff1454_2_690x277.png)

image2798×1124 358 KB

](https://cdn3.ldstatic.com/original/4X/9/1/7/917e0220aea31702561267ccbbf30e6ef0ff1454.png "image")

  

[![image](https://cdn3.ldstatic.com/optimized/4X/c/b/7/cb756f8f79d36ab3f068d06e4eb4046a61b1f051_2_690x301.png)

image2732×1192 378 KB

](https://cdn3.ldstatic.com/original/4X/c/b/7/cb756f8f79d36ab3f068d06e4eb4046a61b1f051.png "image")

[![compass-roadmap-ecosystem.zh](https://cdn3.ldstatic.com/optimized/4X/1/1/5/115c02b72c978ef011f7a245cef437eedb689b40_2_690x460.jpeg)

compass-roadmap-ecosystem.zh1536×1024 375 KB

](https://cdn3.ldstatic.com/original/4X/1/1/5/115c02b72c978ef011f7a245cef437eedb689b40.jpeg "compass-roadmap-ecosystem.zh")

  

[![image](https://cdn3.ldstatic.com/optimized/4X/c/6/7/c67040d729810154885cfa48539645fb3538df7b_2_690x145.png)

image2820×594 138 KB

](https://cdn3.ldstatic.com/original/4X/c/6/7/c67040d729810154885cfa48539645fb3538df7b.png "image")

---

## Comments

> **RioZRon** · [2026-06-16](https://linux.do/t/topic/2416892/2?u=salerihq)
> 
> 怎么感觉着上午发过？是更新了吗 ![:thinking:](https://cdn.ldstatic.com/images/emoji/twemoji/thinking.png?v=15 ":thinking:")

> **sauterne** · [2026-06-16](https://linux.do/t/topic/2416892/3?u=salerihq)
> 
> 被举报删帖了 所以来重新发一下。

> **livingching** · [2026-06-16](https://linux.do/t/topic/2416892/4?u=salerihq)
> 
> 用着HERMES AGENT 加上这个会不会不合适

> **sauterne** · [2026-06-16](https://linux.do/t/topic/2416892/5?u=salerihq)
> 
> 我想做的就是通用的，按理来说应该没啥问题。而且本身也没啥危险行为

> **jisheng** · [2026-06-16](https://linux.do/t/topic/2416892/6?u=salerihq)
> 
> 太厉害了 清华博士 这个用作编程 会越来越懂需求吗?

> **LAbode** · [2026-06-16](https://linux.do/t/topic/2416892/7?u=salerihq)
> 
> 大佬厉害了，最近有个类似困惑和解决想法，大佬已经系统化实现了

> **sauterne** · [2026-06-16](https://linux.do/t/topic/2416892/8?u=salerihq)
> 
> 哈哈哈 也许会，因为大模型本身很懂。他会根据你的任务画像来引导你给出对的回答。  
> 多来几次，你可能就知道应该直接怎么回答了

> **leileio** · [2026-06-16](https://linux.do/t/topic/2416892/9?u=salerihq)
> 
> 博士佬，我想问下，这个skill应对日常普通问题的话，是不是有点大材小用了？感觉这个skill适合用来写论文
> 
> 我就问问天气，改个什么脚本之类，会不会不合适？？

> **sauterne** · [2026-06-16](https://linux.do/t/topic/2416892/10?u=salerihq)
> 
> 我倒是认为它其实很万用呀，比如  
> 
> [![image](https://cdn3.ldstatic.com/optimized/4X/d/1/1/d119eed0e85b65db8766f4d19b1bee9bd41a458e_2_690x389.png)
> 
> image1642×928 105 KB
> 
> ](https://cdn3.ldstatic.com/original/4X/d/1/1/d119eed0e85b65db8766f4d19b1bee9bd41a458e.png "image")

> **xinmei** · [2026-06-16](https://linux.do/t/topic/2416892/11?u=salerihq)
> 
> 诶、跟我想做的星图导轨一样的项目。  
> 本质上就是为了让ai理解我的需求，拆分出各个任务树，类似钢铁雄心任务地图一样。

> **sauterne** · [2026-06-16](https://linux.do/t/topic/2416892/12?u=salerihq)
> 
> 哈哈哈哈 可见我们的需求都是类似的。  
> task-clarifier里面也会拆分需求来着

> **leileio** · [2026-06-16](https://linux.do/t/topic/2416892/13?u=salerihq)
> 
> 佬，那我要是问 这个英语句子 语法正确吗？它也会这么拆分回复吗？？