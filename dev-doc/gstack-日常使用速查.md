# gstack 日常使用速查

> 只保留日常使用相关内容，不含安装步骤。目标是快速判断：现在处于哪个阶段，应该用哪个指令。

## 总流程

```text
想清楚 -> 评审计划 -> 构建 -> 代码评审 -> 浏览器测试 -> 发布 -> 生产验证 -> 复盘/沉淀
```

推荐主线：

1. `/office-hours`：把模糊想法问清楚，重构问题。
2. `/autoplan`：自动跑 CEO、设计、工程、DX 计划评审。
3. 实现功能。
4. `/review`：代码评审，发现生产风险。
5. `/qa`：用真实浏览器测试并修复问题。
6. `/ship`：跑测试、检查覆盖、推送并开 PR。
7. `/land-and-deploy`：合并、等待 CI/部署、验证生产。
8. `/canary`：部署后监控。
9. `/retro`：复盘。
10. `/learn`：沉淀项目经验。

## 阶段速查

| 阶段 | 主要指令 | 什么时候用 | 产出 |
|---|---|---|---|
| 产品想法澄清 | `/office-hours` | 有想法但还不确定做什么、为什么做、先做哪一刀 | 6 个强制问题、问题重构、实现路径、设计文档 |
| CEO 视角评审 | `/plan-ceo-review` | 想确认方向、范围、10x 产品机会 | 范围调整建议、战略取舍 |
| 工程计划评审 | `/plan-eng-review` | 写代码前确认架构、数据流、边界和测试 | 架构图、测试矩阵、失败路径 |
| 设计计划评审 | `/plan-design-review` | UI/体验方案开工前 | 设计评分、AI slop 检查、设计修改建议 |
| DX 计划评审 | `/plan-devex-review` | API、CLI、SDK、文档、开发者产品 | 开发者旅程、TTHW、摩擦点和 DX 方案 |
| 自动完整计划 | `/autoplan` | 想一次性完成 CEO -> 设计 -> 工程 -> DX 评审 | 可执行计划和需要你拍板的 taste 决策 |
| 写精确规格 | `/spec` | 模糊需求需要变成可执行 spec | 经过质量门禁的规格文档 |
| 写精确规格并执行 | `/spec --execute` | spec 通过后直接让新 worktree 执行 | 独立 worktree 中的实现任务 |
| 设计系统 | `/design-consultation` | 从零建立设计方向或设计系统 | `DESIGN.md`、竞品/视觉研究、mockup 方向 |
| 多方案视觉探索 | `/design-shotgun` | 想看 4-6 个视觉方向并迭代 | 对比板、反馈轮次、taste memory |
| Mockup 转 HTML | `/design-html` | 已有 mockup/方案，需要可上线 HTML/CSS | 动态布局、生产级 HTML/CSS/框架代码 |
| 代码评审 | `/review` | 分支有改动，准备测试或发 PR 前 | bug、风险、自动修复、待确认问题 |
| 独立第二意见 | `/codex` | 想让 OpenAI Codex 独立评审/挑战 | pass/fail、对抗式分析、跨模型对比 |
| 根因调查 | `/investigate` | bug 不清楚原因，不能盲修 | 假设、证据、数据流追踪、根因 |
| 浏览器 QA 并修复 | `/qa` | 有 staging/local URL，需要真实点击验证 | bug 修复、回归测试、复验 |
| 只报告 QA | `/qa-only` | 只想要 bug report，不想改代码 | QA 报告 |
| 设计落地审查并修复 | `/design-review` | UI 已经实现，需要设计师视角验收 | 截图、问题、原子修复 |
| DX 实测 | `/devex-review` | 开发者产品已可用，需要真实 onboarding 测试 | TTHW、错误截图、摩擦点报告 |
| 安全审计 | `/cso` | 发版前或安全敏感改动 | OWASP Top 10 + STRIDE 威胁模型、可复现攻击场景 |
| 性能基线 | `/benchmark` | 发版前后比较性能 | 页面加载、Core Web Vitals、资源大小 |
| 发布 PR | `/ship` | 代码准备好，需要测试、覆盖率、推送、PR | 测试结果、覆盖率审计、PR |
| 合并并部署 | `/land-and-deploy` | PR 已批准，需要落到生产 | 合并、CI/部署等待、生产健康验证 |
| 灰度/部署后监控 | `/canary` | 发布后观察生产是否异常 | 控制台错误、性能回退、页面失败 |
| 发布文档更新 | `/document-release` | 功能已发，需要同步 README/架构/贡献文档等 | 文档 diff、Diataxis 覆盖图 |
| 从零生成文档 | `/document-generate` | 缺少教程、how-to、reference、explanation | 新文档 |
| 周期复盘 | `/retro` | 每周/阶段性复盘项目 | 个人/团队产出、测试健康、改进项 |
| 跨项目复盘 | `/retro global` | 想看所有项目和 AI 工具的整体情况 | 全局趋势、shipping streak |
| 项目记忆管理 | `/learn` | 想查看、搜索、清理、导出经验 | 项目 learnings |

## 按场景选评审

| 你在构建什么 | 写代码前 | 上线/实现后 |
|---|---|---|
| 面向终端用户的 UI、Web、移动端 | `/plan-design-review` | `/design-review` |
| 面向开发者的 API、CLI、SDK、文档 | `/plan-devex-review` | `/devex-review` |
| 架构、数据流、性能、测试 | `/plan-eng-review` | `/review` |
| 混合场景，不确定该跑哪个 | `/autoplan` | 视结果补跑对应审查 |

## 浏览器与 QA 指令

| 指令 | 用途 |
|---|---|
| `/browse` | 让 agent 使用真实 Chromium 浏览、点击、截图 |
| `/open-gstack-browser` | 打开带侧边栏、反自动化检测、模型路由的 GStack Browser |
| `/setup-browser-cookies` | 从真实浏览器导入 cookies，用于测试登录态页面 |
| `/pair-agent` | 把浏览器共享给 OpenClaw、Hermes、Codex、Cursor 等其他 agent，每个 agent 独立 tab |
| `$B disconnect` | 从当前浏览器会话断开，回到 headless |
| `$B handoff` | agent 遇到 CAPTCHA、登录、MFA 等卡点时，把同一页面交给你手动处理 |
| `$B resume` | 你处理完浏览器卡点后，让 agent 继续 |
| `$B domain-skill save` | 为当前网站保存一个站点经验，下次访问该 hostname 自动触发 |
| `$B domain-skill promote-to-global` | 把已验证的站点经验提升为跨项目全局经验 |
| `$B cdp <Domain.method>` | 直接调用允许列表中的 Chrome DevTools Protocol 方法 |

## 发布、部署和文档

| 指令 | 用途 |
|---|---|
| `/setup-deploy` | 一次性配置 `/land-and-deploy` 所需的生产 URL、部署平台和命令 |
| `/ship` | 同步主分支、跑测试、审计覆盖率、push、开 PR |
| `/land-and-deploy` | 合并 PR、等待 CI 和部署、验证生产健康 |
| `/canary` | 部署后监控页面错误、性能回退、生产异常 |
| `/document-release` | 根据本次 diff 自动更新已有项目文档 |
| `/document-generate` | 按 Diataxis 生成缺失文档 |

## 安全与编辑保护

| 指令 | 用途 |
|---|---|
| `/cso` | OWASP Top 10 + STRIDE 安全审计 |
| `/careful` | 对 `rm -rf`、`DROP TABLE`、force push、`git reset --hard` 等危险操作给出警告 |
| `/freeze` | 锁定编辑范围到一个目录，防止改到无关文件 |
| `/guard` | 同时启用 `/careful` 和 `/freeze` |
| `/unfreeze` | 解除 `/freeze` 编辑限制 |
| `/investigate` | 根因调查；会自动把编辑范围冻结到正在调查的模块 |

## GBrain 和长期记忆

| 指令/命令 | 用途 |
|---|---|
| `/setup-gbrain` | 初始化持久知识库，可选 Supabase、PGLite、本地/远程 MCP |
| `/setup-gbrain --switch` | 从本地 PGLite 等模式迁移/切换到其他 gbrain 后端 |
| `/sync-gbrain` | 把当前 repo 代码增量索引进 gbrain，并更新 `CLAUDE.md` 搜索指引 |
| `/sync-gbrain --incremental` | 增量同步，默认模式 |
| `/sync-gbrain --full` | 全量重建索引 |
| `/sync-gbrain --dry-run` | 预览同步，不实际写入 |
| `gbrain search` | 在持久知识库中搜索 |
| `gbrain put` | 写入知识到 gbrain |
| `gbrain sources add` | 把当前 repo 注册为 gbrain federated source |
| `gbrain sync --strategy code` | 按代码策略同步 repo |
| `gbrain serve` | 启动 gbrain MCP server |
| `gbrain init` | 初始化 gbrain |
| `gstack-brain-init` | 初始化 gstack memory sync，把 gstack 状态同步到私有 git repo |

每个 repo 的远端信任策略：

| 策略 | 含义 |
|---|---|
| `read-write` | 可搜索，也可从当前 repo 写入新知识 |
| `read-only` | 只能搜索，不能写入 |
| `deny` | 禁止 gbrain 交互 |

## iOS QA 指令

| 指令 | 用途 |
|---|---|
| `/ios-qa` | 通过 USB CoreDevice 驱动真实 iPhone 做 live-device QA |
| `/ios-qa --tailnet` | 通过 Tailscale tailnet 暴露设备给 OpenClaw 或远程 HTTP agent |
| `/ios-fix` | iOS bug 修复循环 |
| `/ios-design-review` | 按 Apple HIG 做 iOS 设计审查 |
| `/ios-clean` | 清理 iOS debug bridge |
| `/ios-sync` | 重新同步 typed accessor |
| `gstack-ios-qa-daemon` | Mac 侧 iOS QA broker，连接 agent 和 USB iPhone |
| `gstack-ios-qa-daemon --tailnet` | 通过 Tailscale 身份门控暴露 iOS QA daemon |
| `gstack-ios-qa-mint grant` | 给远程 agent 授权 iOS QA allowlist |
| `gstack-ios-qa-mint revoke` | 撤销远程 agent 的 iOS QA 授权 |
| `gstack-ios-qa-mint list` | 查看 iOS QA allowlist |

iOS 能力层级：

| 层级 | 含义 |
|---|---|
| `observe` | 只能观察 |
| `interact` | 可交互 |
| `mutate` | 可改状态 |
| `restore` | 可恢复状态 |

## OpenClaw 使用模板

在 OpenClaw agent 中，可以用自然语言触发下面这些工作流：

| 你说 | 发生什么 |
|---|---|
| `Fix the typo in README` | 简单任务，直接开 Claude Code session，不一定需要 gstack |
| `Run a security audit on this repo` | 用 `Run /cso` 启动安全审计 |
| `Build me a notifications feature` | 用 `/autoplan -> implement -> /ship` 完成从计划到 PR |
| `Help me plan the v2 API redesign` | 用 `/office-hours -> /autoplan` 生成并保存计划，不实现 |

也可以明确给 OpenClaw 这些指令：

| 任务 | 模板 |
|---|---|
| 安全审计 | `Load gstack. Run /cso` |
| 代码评审 | `Load gstack. Run /review` |
| QA 测试 URL | `Load gstack. Run /qa https://...` |
| 端到端构建功能 | `Load gstack. Run /autoplan, implement the plan, then run /ship` |
| 只做计划不实现 | `Load gstack. Run /office-hours then /autoplan. Save the plan, don't implement.` |

OpenClaw 原生会话技能：

| 技能 | 用途 |
|---|---|
| `gstack-openclaw-office-hours` | 用 6 个强制问题做产品追问 |
| `gstack-openclaw-ceo-review` | 用 4 种范围模式做战略挑战 |
| `gstack-openclaw-investigate` | 根因调试方法论 |
| `gstack-openclaw-retro` | 周工程复盘 |

## 独立 CLI 和配置命令

| 命令 | 用途 |
|---|---|
| `gstack-model-benchmark` | 用同一 prompt 横向比较 Claude、GPT/Codex CLI、Gemini 的延迟、tokens、成本和质量 |
| `gstack-model-benchmark --dry-run` | 只验证参数和鉴权，不花 API 调用 |
| `gstack-taste-update` | 把 `/design-shotgun` 的喜欢/不喜欢写入项目 taste profile |
| `gstack-config set checkpoint_mode continuous` | 开启连续 checkpoint，本地自动 `WIP:` 提交 |
| `gstack-config set telemetry off` | 关闭遥测 |
| `gstack-analytics` | 查看本地 JSONL 使用统计，不需要远程遥测 |

运行时环境变量/配置：

| 配置 | 用途 |
|---|---|
| `GSTACK_SECURITY_ENSEMBLE=deberta` | 启用 721MB DeBERTa-v3 prompt injection 防护 ensemble |
| `GSTACK_SECURITY_OFF=1` | 紧急关闭 prompt injection 防护 |
| `GSTACK_ANTHROPIC_API_KEY` | Conductor 环境中给 gstack TS 入口点使用的 Anthropic key |
| `GSTACK_OPENAI_API_KEY` | Conductor 环境中给 gstack TS 入口点使用的 OpenAI key |
| `auto_upgrade: true` | 在 `~/.gstack/config.yaml` 中启用自动升级检查 |

连续 checkpoint 相关：

| 配置/指令 | 用途 |
|---|---|
| `checkpoint_mode continuous` | 技能执行中持续本地 WIP commit |
| `checkpoint_push=true` | 允许把 checkpoint push 到远端；默认不 push |
| `/context-restore` | 从 WIP commit 的 `[gstack-context]` 恢复会话上下文 |
| `/ship` | 发 PR 前会 filter-squash WIP commits，保留非 WIP commit |

## 语音友好触发短语

| 你可以说 | 对应倾向 |
|---|---|
| `run a security check` | `/cso` |
| `test the website` | `/qa` 或 `/qa-only` |
| `do an engineering review` | `/plan-eng-review` 或 `/review` |
| `do a design review` | `/plan-design-review` 或 `/design-review` |
| `help me plan this` | `/office-hours` 或 `/autoplan` |
| `investigate this bug` | `/investigate` |
| `ship this` | `/ship` |

## 全部指令清单

### Slash commands

```text
/office-hours
/plan-ceo-review
/plan-eng-review
/plan-design-review
/design-consultation
/design-shotgun
/design-html
/review
/ship
/land-and-deploy
/canary
/benchmark
/browse
/connect-chrome
/open-gstack-browser
/qa
/qa-only
/design-review
/setup-browser-cookies
/setup-deploy
/setup-gbrain
/setup-gbrain --switch
/sync-gbrain
/sync-gbrain --incremental
/sync-gbrain --full
/sync-gbrain --dry-run
/retro
/retro global
/investigate
/document-release
/document-generate
/codex
/cso
/autoplan
/plan-devex-review
/devex-review
/pair-agent
/careful
/freeze
/guard
/unfreeze
/gstack-upgrade
/learn
/spec
/spec --execute
/ios-qa
/ios-qa --tailnet
/ios-fix
/ios-design-review
/ios-clean
/ios-sync
/context-restore
```

如果安装时启用了 namespaced commands，slash 指令会带 `gstack-` 前缀，例如 `/gstack-qa`；如果使用无前缀模式，则是 `/qa`。

### Browser `$B` commands

```text
$B disconnect
$B handoff
$B resume
$B domain-skill save
$B domain-skill promote-to-global
$B cdp <Domain.method>
```

### Standalone binaries / shell commands

```text
gstack-model-benchmark
gstack-model-benchmark --dry-run
gstack-taste-update
gstack-ios-qa-daemon
gstack-ios-qa-daemon --tailnet
gstack-ios-qa-mint grant
gstack-ios-qa-mint revoke
gstack-ios-qa-mint list
gstack-config set checkpoint_mode continuous
gstack-config set telemetry off
gstack-analytics
gstack-brain-init
gbrain init
gbrain serve
gbrain search
gbrain put
gbrain sources add
gbrain sync --strategy code
```

## 最常用组合

### 从 0 到 PR

```text
/office-hours
/autoplan
实现功能
/review
/qa <你的 staging 或 local URL>
/ship
```

### 只想评审一个已有分支

```text
/review
/codex
/qa-only <URL>
```

### UI 功能

```text
/plan-design-review
实现 UI
/design-review
/qa
/ship
```

### 开发者工具、API、SDK、文档

```text
/plan-devex-review
实现功能
/devex-review
/document-release
/ship
```

### 安全敏感改动

```text
/plan-eng-review
/cso
/review
/qa
/ship
```

### 发布后

```text
/land-and-deploy
/canary
/document-release
/retro
/learn
```
