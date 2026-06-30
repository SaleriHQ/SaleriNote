---
title: "「开源」是时候精简你的 AGENTS.md 了! By渐进式披露"
source: "https://linux.do/t/topic/2263095"
author:
  - "[[Caphhh]]"
published: 2026-05-28
created: 2026-06-21
description: "本帖使用社区开源推广，符合推广要求。我申明并遵循社区要求的以下内容： 我的帖子已经打上 开源推广 标签： 是 我的开源项目完整开源，无未开源部分： 是 我的开源项目已链接认可 LINUX DO 社区： 是 我帖子内的项目介绍，AI生成、润色内容部分已截图发出： 是 以上选择我承"
tags:
  - "clippings"
---
#### 本帖使用社区开源推广，符合推广要求。我申明并遵循社区要求的以下内容：

- **我的帖子已经打上 [开源推广](https://linux.do/tag/2234-tag/2234) 标签：** 是
- **我的开源项目完整开源，无未开源部分：** 是
- **我的开源项目已链接认可 LINUX DO 社区：** 是
- **我帖子内的项目介绍，AI生成、润色内容部分已截图发出：** 是
- **以上选择我承诺是永久有效的，接受社区和佬友监督：** 是

*以下为项目介绍正文内容，AI生成、润色内容已使用截图方式发出*

---

[github.com](https://github.com/Caph-dev/agents-progressive-disclosure)

![](https://cdn3.ldstatic.com/optimized/4X/3/6/e/36e5a10925fb7014467cb034bd7626ae48e52f76_2_690x344.png)

### [GitHub - Caph-dev/agents-progressive-disclosure: A skill to refactor bloated AGENTS.md, CLAUDE.md,...](https://github.com/Caph-dev/agents-progressive-disclosure)

A skill to refactor bloated AGENTS.md, CLAUDE.md, or similar agent instruction files into a compact routing entrypoint plus focused docs/ reference files.

**安装：**  
`npx skills add Caph-dev/agents-progressive-disclosure`  
或者：把上面的链接发给你的 agent 让它装

**使用方法：**

```plaintext
使用 $agents-progressive-disclosure，把当前 AGENTS.md 重构成精简入口文件和 docs/ 专项文档。
```

**在用完这个 skill 之后，务必亲自检查一遍。你可以问 agent 当前策略会不会区分得太细？有些常用流程的是不是没必要放到 docs 下？**

流程（示意图仅供流程参考，图中的文档分类并不是一个好的示例）：  

[![PixPin2026-05-2819-52-11](https://cdn3.ldstatic.com/optimized/4X/8/8/5/885a55dde58a6e4de11322f1f6c6060b7469c288_2_690x414.jpeg)

PixPin2026-05-2819-52-111920×1152 194 KB

](https://cdn3.ldstatic.com/original/4X/8/8/5/885a55dde58a6e4de11322f1f6c6060b7469c288.jpeg "PixPin2026-05-2819-52-11")

---

好处：

1. AGENTS.md 保持精简
2. 省上下文
3. 技术文档可以写得非常详细，因为只有在需要时才加载。（当然，你也可以把技术文档提炼为 skill，类似的效果）
4. 更容易维护

我的 codex 的全局 AGENTS.md 示例：

全局 AGENTS.md 示例
```markdown
# 全局 Agent 指令

> 适用范围：本文件是全局代理行为入口。这里只保留必须常驻的规则；任务细节按需读取 \`docs/\` 下的专项文档。

## 常驻原则

- 当前环境按 **macOS 原生环境 + Ghostty + zsh** 处理；不要默认使用 Windows、PowerShell、Linux 发行版专属命令或 GNU coreutils 特有行为。
- 需要当前信息、URL 正文、官方文档、API/SDK/框架文档、配置步骤、版本迁移或多来源核验时，默认使用本机 \`smart-search\` CLI。
- 不要仅凭训练数据回答 API 细节、配置项、版本相关行为或可能变化的事实。
- 新闻、政策、财经、医疗、法律、安全、严肃评测、工具选型、价格、强时效信息和高风险结论，必须先抓取关键正文再下结论。
- 不执行破坏性命令、\`sudo\`、历史重写或清理命令，除非用户明确要求；删除前必须确认目标路径。
- \`AGENTS.md\` 是路由入口，不是规则仓库；只读取与当前任务相关的专项文档，不要一次性加载全部 \`docs/*.md\`。

## 常驻输出规则

- 默认使用中文回答；用户明确要求英文，或代码、API、命令、错误信息、专有名词本身需要保留英文时，可以保留英文。
- 执行命令前，简要说明命令目的。
- 命令失败时，报告执行了什么命令、关键错误信息、下一步修复方案。
- 不把 Windows PowerShell 命令或 Linux 发行版专属命令直接搬到 macOS zsh / Ghostty 中运行。
- 回答事实性结论时，区分“已验证”“候选来源”“未验证推断”。
- 用户只要求简要回答时，直接给结论，不主动展开背景。
- 用户要求文档、配置或代码落地时，优先直接修改相关文件并验证，不停留在建议层。

## 按需读取索引

| 任务类型                                                     | 先读文档                      | 触发条件                                                     |
| ------------------------------------------------------------ | ----------------------------- | ------------------------------------------------------------ |
| 联网搜索、网页抓取、官方文档、API/SDK/库用法、Deep Research  | \`docs/search-and-evidence.md\` | 需要当前信息、来源链接、URL 正文、库文档、政策/新闻/高风险事实核验 |
| 本地环境、Shell、路径、文件搜索、环境变量、Homebrew、Ghostty | \`docs/local-environment.md\`   | 需要运行命令、改 shell 配置、查路径、安装命令行工具、操作终端配置 |
| 项目命令、包管理器、Python/Node、Git、安全边界               | \`docs/project-workflow.md\`    | 需要构建、测试、安装依赖、改项目、提交代码、删除/移动文件    |

更多导航见 \`docs/README.md\`。

## 常驻安全边界

- 不把未验证变量传给 \`rm -rf\`；不执行 \`git reset --hard\`、\`git clean -fdx\` 等破坏性 Git 命令，除非用户明确要求。
- 不默认修改 \`~/.zshenv\`；Shell、Homebrew、Ghostty 和路径细则见 \`docs/local-environment.md\`。
- 运行项目命令前先检查项目文件和已有 scripts；包管理器、运行时和 Git 细则见 \`docs/project-workflow.md\`。
- Node 包管理器按锁文件选择；如果存在多个锁文件，不擅自选择，先说明冲突并询问用户。
- 如果工作区已有与当前任务无关的改动，不要回滚；只处理当前任务范围。
- \`smart-search doctor --format json\` 只在检索失败、排查配置或用户明确要求检查可用性时运行。

## 优先级

1. 用户当前明确指令。
2. 当前项目或更近目录的 \`AGENTS.md\`。
3. 本文件。
4. 本文件路由到的 \`docs/*.md\` 细则。

如果专项文档与本入口文件冲突，以本入口文件的常驻规则为准；如果用户明确覆盖，以用户当前指令为准。
```

---

## Comments

> **SuliWang** · [2026-05-28](https://linux.do/t/topic/2263095/3?u=salerihq)
> 
> 其实有的时候感觉这个agent.md有的时候跟moe一样，路由不同的专家。