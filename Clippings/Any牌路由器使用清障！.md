---
title: "Any牌路由器使用清障！"
source: "https://linux.do/t/topic/1779614"
author:
  - "[[Haleclipse]]"
published: 2026-03-18
created: 2026-06-21
description: "3s内报403 你就等 别瞎琢磨哪配错了 L站都快成Any牌路由器反馈地了 只要一波动一报错就有人问 “我不行 你行不行 你咋配的 为啥我不行 看看你配置 我4xx/5xx 我屯屯鼠 我是猪” 哇脑阔噶痛啊 某日 某 快问快答 下的系统推荐帖列表 （竟恐怖如斯） image11"
tags:
  - "clippings"
---
## 3s内报403 你就等 别瞎琢磨哪配错了

---

L站都快成Any牌路由器反馈地了  
只要一波动一报错就有人问  
“我不行 你行不行 你咋配的 为啥我不行 看看你配置 我4xx/5xx 我屯屯鼠 ~~我是猪~~”  
哇脑阔噶痛啊

某日 某 [快问快答](https://linux.do/tag/1436-tag/1436) 下的系统推荐帖列表 （竟恐怖如斯）  

[![image](https://cdn3.ldstatic.com/optimized/4X/b/d/d/bdd6f911c78dc3b6ea4deff7a62290c6b16be110_2_681x500.jpeg)

image1116×819 127 KB

](https://cdn3.ldstatic.com/original/4X/b/d/d/bdd6f911c78dc3b6ea4deff7a62290c6b16be110.jpeg "image")

现在！立刻！全方位！排除所有疑问 写一个说明书  
必须傻瓜版 多问一句都叉(我)出去！

## 基础要点

*不讲且不用 CCS 等辅助编辑工具*

从未使用过CC  
新安装 CC 后， 是没有 `~/.claude/settings.json` 文件的 (安装指南请看 [《 Claude Code 终极版 FAQ 指南 》](https://linux.do/t/topic/803265))  
同时 还可能被 **登录引导** 过程 拦截  
受 `~/.claude.json` 中顶层开关项 `hasCompletedOnboarding` 所控制和记录状态  
顾名思义 `false` 即为 登录引导没有过 需要显示  
反之 `true` 即为 已经经过 登录引导 无须再显示  
所以 作为 官方服务来说 本就没有所谓的第三方API支持服务 弹出登录引导是正确且合理的行为设计  
格局上来说 中国大陆不在服务支持范围 Claude Code 充其量就是一个客户端(发包器) 没有什么魔法  
什么花都可以改出来，什么都能搓成你想要的样子 只要花点时间 （可以说是半开源）

此处显而易见的 你不使用官方规定提供的服务 直接打开 `~/.claude.json` 搜索 `hasCompletedOnboarding`  
如存在则直接修改值成true 如不存在则自行追加到JSON对象顶层

~/.claude.json 傻瓜对照示例

处于尾行无须逗号，处于中间不能缺少逗号

```json
{
  "hasCompletedOnboarding": true,
}
```
登录引导图示

有两种情况： （默认你处于中国大陆）

- 没在终端吃到代理配置 访问端点`api.anthropic.com` 不通 就是以下内容  
	[![image](https://cdn3.ldstatic.com/optimized/4X/c/c/6/cc69de4c7a6ddbc34971996f66e0115923eba532_2_690x455.jpeg)
	image1538×1016 196 KB
	](https://cdn3.ldstatic.com/original/4X/c/c/6/cc69de4c7a6ddbc34971996f66e0115923eba532.jpeg "image")
- 在终端吃到代理配置了 访问端点 `api.anthropic.com` 成功了 就是以下内容  
	（注： 无论你使用的什么手段让其通了 比如我这里的export代理变量 最终结果差异就两种）  
	[![image](https://cdn3.ldstatic.com/optimized/4X/6/0/e/60ec6d399e48bae95a8fe449b4711f166945bf21_2_579x499.jpeg)
	image1920×1658 324 KB
	](https://cdn3.ldstatic.com/original/4X/6/0/e/60ec6d399e48bae95a8fe449b4711f166945bf21.jpeg "image")

准备工作完成 能够正常进入看到CC的Prompt输入框  

[![image](https://cdn3.ldstatic.com/optimized/4X/6/b/d/6bdd0b283f2cf5fb3a2d72e8705b016a540dd862_2_689x203.jpeg)

image1196×352 71.6 KB

](https://cdn3.ldstatic.com/original/4X/6/b/d/6bdd0b283f2cf5fb3a2d72e8705b016a540dd862.jpeg "image")

接下来是 供应商管理和必要的基础开关项 (国情如此 不得不要)

以空的不存在的 `~/.claude/settings.json` 为例进行最小化讲解  
(不存在就新建！别问为什么没有，没有触发到创建的逻辑自然没有)

> ~/.claude/settings.json
> 
> ```json
> {
>  "env": {
>    "HTTPS_PROXY": "http://127.0.0.1:7890",
>    "HTTP_PROXY": "http://127.0.0.1:7890",
>    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
>    "CLAUDE_CODE_ATTRIBUTION_HEADER": "0",
>    "DISABLE_INSTALLATION_CHECKS": "1",
>    "ENABLE_TOOL_SEARCH": "1"
>  },
>  "cleanupPeriodDays": 720,
>  "model": "opus[1m]"
> }
> ```

env对象属性值全部必须写为**字符串** 而不是布尔值或者其他！  
字符串内容通常使用 “1”=“yes”=“on” / “0”=“no”=“false”  
为统一化 直接写 0/1 通用百分百错不了

- `HTTP_PROXY` / `HTTPS_PROXY` ： 对CC指定代理服务器配置 一劳永逸的常规配置项 告别无脑 tun tun
- `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` ：详见 [用CC，这个应该写进默认配置](https://linux.do/t/topic/1365615)
	- 该项为复合设置项 同时包揽：
		- `DISABLE_AUTOUPDATER` 禁用自动升级
				- `DISABLE_BUG_COMMAND` 禁用显示 `/bug` 反馈 命令
				- `DISABLE_ERROR_REPORTING` 禁用错误报告行为
				- `DISABLE_TELEMETRY` 禁用遥测！
		- 可以见得 实际上这里面最起作用的是 禁用遥测 这一开关项 但我们国情如此 另外两项附带的都没啥屁用 所以一包四 挺好~ 而自动升级也不是特别必要 因为自主控制升降级 也挺好
		- 如果你需要 自动升级 那么只需让该项代换成 1项 → 3项 不写 `DISABLE_AUTOUPDATER` 多写另外三项 即可达成
- `CLAUDE_CODE_ATTRIBUTION_HEADER`： 是否追加客户端类型区分文本 默认值是追加 详见：[出结论了，Claude Code 新版本随机插入 cch 参数，第三方缓存命中率从 90% 降至 30% - #9，来自 Haleclipse](https://linux.do/t/topic/1613608/9)
	- 该变量导出来控制是否不需要装填 billing header 到 请求体中 (默认不写就是会装填 所以写成"0"，因为该值根本对我们根本没有什么作用)
		- 该内容形如 `{"anthropic-billing-header: cc_version=2.1.15.c7e; cc_entrypoint=cli"}`
- `DISABLE_INSTALLATION_CHECKS`： 禁用安装方式检查 详见：[A\\ 又疯了](https://linux.do/t/topic/1498076) / [区别详细解释](https://linux.do/t/topic/1422264/22)
	- 最直观的就是略过检查 以去除 仍在使用所谓遗弃的 npm安装方式所被提示的 黄色警告文本显示
- `ENABLE_TOOL_SEARCH`： 启用ToolSearch这一Tool，该Tool的能力是将Tool们全部延后加载 在需要时会自动先调用该工具进行Tool的搜索加载 （因为默认是直接加载到上下文窗口的）
	- 可加可不加 最好不写 因为在 1M上下文窗口实际上不是特别有必要了
		- 且实际上 Sonnet / Opus 4+ 才支持这种能力
		- 为什么和什么时候需要显式指定？ 详见： [CC环境变量 搞点简单记录](https://linux.do/t/topic/1513988)
		- 而且Anyrouter上游账号不知道会打到哪批？很有可能并不能被支持

以下是非env 设置类开关项

- `cleanupPeriodDays` 用于将默认三十天删除会话记录这一时间覆盖更长 意为不需要删除旧会话 详见 ： [啥毙CC 不吐不快](https://linux.do/t/topic/1618652)
- `model` 该项同样属于可写可不写 因为REPL交互式TUI中的 `/model` 选择器会默认的将选择值写到这里 所以锁全局的意义不大（如果常更换模型的话）默认值：`default`
	- 且此模块设计有别名系统 即 opus 早期版本可能可以指代 `claude-opus-4-5-20251101` 而并不需要去记忆具体的模型ID 当前版本为4.6 则 opus 会指代 `claude-opus-4-6`
		- 默认值别名 default 在不同账户情况下 可能会指代给 sonnet别名 或者 opus别名 具体以打开的 列表项显示为准

全局的 `~/.claude/settings.json` 基础开关控制项 说完 接下来就是 三方提供商设定  
此处依赖 `--settings` cli参数来载入更高优先级的 Profile 覆盖值 以达成灵活的供应商切换和管理, 直接创建 `~/.claude/settings.xxxx.json`

以下以 anyr0uter 为例进行讲解  
创建 `~/.claude/settings.anyrouter.json`

> ~/.claude/settings.anyrouter.json
> 
> ```json
> {
>  "env": {
>    "ANTHROPIC_BASE_URL": "https://anyrouter.top",
>    "ANTHROPIC_AUTH_TOKEN": "you-token-to-here"   
>  },
>  "model": "opus[1m]"
> }
> ```

应该写什么为什么这样写的 基础逻辑就是 `--settings` 参数为 整合全局内容覆盖项  
只要我们不在全局中写 就可以调整到 Profile (provider) 中写

所以 这就可以解释 为什么 末尾 要写 “model” 这项模型别名指定  
因为Any牌路由器已经很久不提供 Sonnet 模型了  
所以根本无须考虑 切换使用 Sonnet模型 则直接锁定在推荐的 1M 模型上  
但这只针对使用Anyrouter时 不会影响其他的Profile 非常灵活和方便

接下来再以一个 特别的案例对另一块迷惑误区点进行讲解：

创建 `~/.claude/settings.aliyun.json`

> ~/.claude/settings.aliyun.json
> 
> ```json
> {
>  "env": {
>    "ANTHROPIC_BASE_URL": "https://dashscope.aliyuncs.com/apps/anthropic",
>    "ANTHROPIC_API_KEY": "you-token-to-here",
>    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "qwen3.5-flash",
>    "ANTHROPIC_DEFAULT_SONNET_MODEL": "qwen3.5-flash",
>    "ANTHROPIC_DEFAULT_OPUS_MODEL": "qwen3.5-plus"
>  }
> }
> ```

可以看到 我们在此处为什么会去设定 默认模型路由 而 Anyrouter 不需要？  
因为Any牌路由器 提供了 `claude-haiku-4-5-20251001` 的模型id 这与官方完全匹配  
包括 `claude-opus-4-6` 也是与官方匹配 当然就不需要去覆盖指定

而使用阿里云百炼的时候 模型提供商并没有提供 haiku / sonnet / opus 对等的模型id  
只是模型能力体量上我们知道且可以控制其代换的模型  
所以把高能力模型id塞给 opus  
快速且费率低的模型id塞给 haiku  
sonnet无所谓 随意指定

最后 我们撰写完成之后 就可以使用  
`claude --settings ~/.claude/settings.xxxx.json`  
加载打开不同的Profile (provider) 来使用不同的提供商配置  
同样 末尾仍可以 追加 `-r` `-c` 等参数在载入不同的供应商配置时 选择继续上次会话 还是 恢复某个会话

Windows用户不理解 `~` (家目录指代) 的 请看[Claude Code 使用原生 \`--settings\` 选项配置多个自定义模型供应商](https://linux.do/t/topic/957691) 加以理解

Q：此处的BASE\_URL 应该怎么填 格式怎样  
A：CC默认会追加 `/v1/messages` 拼接 进行 LLM请求，所以 BASE\_URL 就是指 抠掉这块之前的那部分 动动你的小脑瓜 不要想当然无脑 往上自己追加 /v1 后缀

Q: API无法连接 （API Error: Unable to connect to API (ECONNRESET)）？  
A: 佬铁 核查网络抵达性啦 访问不到臣妾也没招w

Q: 我怎么知道供应商提供了哪些模型ID呢？  
A: 通常呢 供应商会有一个 `/v1/models` 的端点，只要 GET请求 附带你的token到请求头中，打过去就能得到 一个 json结果 里面就是 提供的模型ID

Q: 响应码 4xx （no body）?  
A: 切模型到 Opus 1M （盲猜Opus没米了）

- 就算提问也不要光说自己得到了什么响应码错误，没有响应码之神 可以透过响应码贯穿得知响应的message，这很重要
- 那么如何得知message呢 POST打一下也就知道了 如果有的话 比如现在没有米的情况是  
	[![image](https://cdn3.ldstatic.com/optimized/4X/f/f/a/ffa5634bc91330c55851376c2b37709ae1cffec6_2_690x285.png)
	image1408×582 122 KB
	](https://cdn3.ldstatic.com/original/4X/f/f/a/ffa5634bc91330c55851376c2b37709ae1cffec6.png "image")

Q: 用着用着弹xxx中断了是为何？  
A: 存在这么一种情况 Haiku模型他欠费了但主模型Opus是能通的，某时刻乍一上来的时候 通过 `-c` 继续会话 并不会触发 Haiku 调用 继续单独打到Opus上 则不会有什么问题  
但是一旦触及到Haiku调用 就boom了。他很有可能是这种情况

---

以上全部为手脑并用撰写，零LLM参与  
如果对你有一点帮助，点击我的头像点我一个 **关注**  
下回能再见也说不定呢w

接下来会登场的是

[![image](https://cdn3.ldstatic.com/optimized/4X/5/4/f/54fc3dc204f86a1a143c08182ed9bcd74a8b2fea_2_500x500.jpeg)

image1024×1024 146 KB

](https://cdn3.ldstatic.com/original/4X/5/4/f/54fc3dc204f86a1a143c08182ed9bcd74a8b2fea.jpeg "image")