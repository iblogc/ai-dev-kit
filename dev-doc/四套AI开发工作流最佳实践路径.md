# 四套 AI 开发工作流最佳实践路径

> 综合 `gstack`、`Superpowers`、`GSD Core`、`Skills For Real Engineers` 四套方法论，按日常场景给出最佳使用路径。文件名沿用“三套”是为了兼容已有文档引用。

目标不是四套都硬跑一遍，而是在正确阶段选正确工具：

```text
找需求 -> 澄清语言 -> 规格/路线图 -> 计划 -> TDD/执行 -> 评审 -> QA/UAT -> 发布 -> 复盘/沉淀
```

## 一句话定位

| 工具 | 最适合解决什么 | 关键词 |
|---|---|---|
| `gstack` | 像虚拟工程团队一样补齐产品、设计、工程、QA、安全、发布角色 | 产品评审、设计、QA、浏览器、安全、发布 |
| `Superpowers` | 强制 agent 遵守可靠软件工程流程，尤其是 TDD 和代码评审 | 先设计、再计划、TDD、worktree、评审 |
| `GSD Core` | 用规格驱动和上下文工程解决大项目 context rot，让长周期开发稳定推进 | phase、roadmap、context、wave 执行、里程碑 |
| `Skills For Real Engineers` | 小而可组合的工程基本功，保留你的控制权 | grill、共享语言、PRD、issues、TDD、diagnose、架构 |

## 总体选择原则

| 你现在的问题 | 首选 | 辅助 |
|---|---|---|
| 不知道该做什么、需求还模糊 | `gstack /office-hours` | `Skills /grill-me`、`Superpowers brainstorming` |
| 已经知道要改代码，但怕 agent 理解错 | `Skills /grill-with-docs` | `gstack /office-hours` |
| 需要把需求写成 PRD 或 issue | `Skills /to-prd`、`/to-issues` | `GSD /gsd-new-project` |
| 已经有目标，需要整理成路线图 | `GSD /gsd-new-project` | `gstack /autoplan` |
| 大项目、长周期、多阶段 | `GSD Core` | `gstack` 做专项评审 |
| 小功能、想稳妥实现 | `Skills /tdd` 或 `Superpowers test-driven-development` | `gstack /review`、`/qa` |
| 代码库术语混乱、agent 太啰嗦 | `Skills /grill-with-docs` | `Skills /caveman` |
| UI/产品体验需要打磨 | `gstack` | `GSD /gsd-ui-phase`、`Skills /prototype` |
| 代码质量和 TDD 纪律 | `Skills /tdd` | `Superpowers test-driven-development` |
| 难 bug 或性能回退 | `Skills /diagnose` | `gstack /investigate`、`GSD /gsd-debug` |
| 浏览器真实测试 | `gstack /qa` | `GSD /gsd-verify-work` |
| 发布 PR 和生产验证 | `gstack /ship`、`/land-and-deploy` | `GSD /gsd-ship` |
| 防止长上下文退化 | `GSD Core` | `Skills /handoff` |
| 代码库开始变成泥球 | `Skills /improve-codebase-architecture` | `gstack /review` |

## 推荐总工作流

### 重要功能或新项目

```text
# 需求与语言
gstack /office-hours
Skills /grill-with-docs

# 路线图与阶段
GSD /gsd-new-project
GSD /gsd-discuss-phase 1
gstack /autoplan
GSD /gsd-plan-phase 1

# 任务和执行
Skills /to-prd
Skills /to-issues
Superpowers using-git-worktrees
GSD /gsd-execute-phase 1
Skills /tdd

# 质量
gstack /review
gstack /codex
gstack /qa
GSD /gsd-verify-work 1

# 发布与沉淀
gstack /ship
gstack /land-and-deploy
gstack /canary
gstack /document-release
gstack /retro
Skills /handoff
```

### 更实际的简化版

```text
gstack /office-hours
Skills /grill-with-docs
GSD /gsd-new-project
GSD /gsd-discuss-phase 1
GSD /gsd-plan-phase 1
GSD /gsd-execute-phase 1
gstack /review
gstack /qa
GSD /gsd-verify-work 1
gstack /ship
```

### 不想被完整流程接管时

```text
Skills /grill-with-docs
Skills /to-prd
Skills /to-issues
Skills /tdd
gstack /review
gstack /qa
```

## 场景 1：找需求 / 找真正问题

目标：从“我想做一个功能”变成“我知道用户痛点、范围、优先级和成功标准”。

推荐路径：

```text
gstack /office-hours
Skills /grill-me
Superpowers brainstorming
GSD /gsd-new-project
```

怎么用：

| 步骤 | 工具 | 目的 |
|---|---|---|
| 1 | `gstack /office-hours` | 用 YC office hours 式追问重新定义问题 |
| 2 | `Skills /grill-me` | 对非代码计划做 relentless interview，补齐决策树 |
| 3 | `Superpowers brainstorming` | 用苏格拉底式问题把粗想法变成可确认设计 |
| 4 | `GSD /gsd-new-project` | 生成 `PROJECT.md`、`REQUIREMENTS.md`、`ROADMAP.md`、`STATE.md` |

最佳实践：

| 做 | 不做 |
|---|---|
| 先讲痛点和真实例子 | 一上来让 agent 写代码 |
| 明确 v1、v2、非范围 | 把所有想法都塞进第一版 |
| 让 agent 反驳你的假设 | 只让 agent 顺着你说 |
| 需求稳定后再写 PRD | 没问清楚就拆 issue |

## 场景 2：改代码前对齐 / 建立共享语言

目标：避免 agent 误解项目术语、用一大段话描述一个领域概念、命名不一致。

推荐路径：

```text
Skills /setup-matt-pocock-skills
Skills /grill-with-docs
Skills /to-prd
Skills /to-issues
```

`/grill-with-docs` 适合每次代码改动前使用。它会挑战你的计划、对齐现有 domain model，并更新：

```text
CONTEXT.md
docs/adr/
```

什么时候优先用它：

| 情况 | 原因 |
|---|---|
| 项目里有很多业务术语 | 建立共享语言能减少后续 token 和误解 |
| agent 输出太啰嗦 | 用术语替代长解释 |
| 需要新增模块或改核心流程 | ADR 能记录关键决策 |
| 想保留人工控制权 | 它只增强局部流程，不接管整个项目 |

## 场景 3：头脑风暴 / 产品方向探索

目标：比较不同方案，找到更小、更准、更值得做的版本。

产品方向：

```text
gstack /office-hours
gstack /plan-ceo-review
gstack /autoplan
```

工程可行性：

```text
Skills /grill-with-docs
GSD /gsd-discuss-phase 1 --analyze
gstack /plan-eng-review
```

UI/体验探索：

```text
gstack /plan-design-review
gstack /design-consultation
gstack /design-shotgun
Skills /prototype
```

怎么选：

| 你想探索什么 | 推荐 |
|---|---|
| 产品是否值得做 | `gstack /plan-ceo-review` |
| 需求分支是否都问清楚 | `Skills /grill-me` |
| 有哪些实现方案 | `GSD /gsd-discuss-phase --analyze` |
| 架构是否合理 | `gstack /plan-eng-review` |
| 视觉方向 | `gstack /design-shotgun`、`Skills /prototype` |
| 开发者体验 | `gstack /plan-devex-review` |

## 场景 4：已有代码库新增功能

目标：让 agent 先理解现有系统和语言，再规划新增功能，避免凭空发明架构。

推荐路径：

```text
GSD /gsd-map-codebase
Skills /grill-with-docs
GSD /gsd-new-project
GSD /gsd-discuss-phase 1
GSD /gsd-plan-phase 1 --reviews
Superpowers using-git-worktrees
GSD /gsd-execute-phase 1
```

增强质量：

```text
Skills /tdd
gstack /plan-eng-review
gstack /review
Skills /zoom-out
```

最佳实践：

| 做 | 原因 |
|---|---|
| 先 `/gsd-map-codebase` | 避免 agent 无视现有约定 |
| 再 `/grill-with-docs` | 让 agent 使用项目共享语言 |
| 用 `/gsd-plan-phase --reviews` | 把代码库审查结果纳入计划 |
| 用 worktree | 保持主工作区干净 |
| 每阶段做 `/gsd-verify-work` | 自动测试不等于功能符合预期 |

## 场景 5：从 0 开发新项目

目标：让 AI 长周期稳定推进，而不是上下文越堆越烂。

推荐路径：

```text
gstack /office-hours
Skills /grill-me
GSD /gsd-new-project
GSD /gsd-discuss-phase 1
GSD /gsd-plan-phase 1
GSD /gsd-execute-phase 1
Skills /tdd
GSD /gsd-verify-work 1
gstack /qa
gstack /ship
```

每个后续 phase：

```text
GSD /gsd-discuss-phase 2
GSD /gsd-plan-phase 2
GSD /gsd-execute-phase 2
GSD /gsd-verify-work 2
gstack /qa
gstack /ship
```

里程碑完成：

```text
GSD /gsd-audit-milestone
GSD /gsd-complete-milestone
GSD /gsd-new-milestone
```

为什么这样组合：

| 工具 | 作用 |
|---|---|
| `gstack` | 前期产品判断、后期 QA/发布 |
| `Skills` | 共享语言、PRD、issues、TDD、诊断 |
| `GSD Core` | 中间长周期规格、计划、执行、上下文管理 |
| `Superpowers` | 如果你想强制 worktree、小步评审，可穿插使用 |

## 场景 6：小功能 / 小修小改

目标：别上完整重流程，但也不要裸奔。

最快路径：

```text
GSD /gsd-fast <text>
```

稳妥路径：

```text
Skills /grill-with-docs
GSD /gsd-quick --discuss --research --full
gstack /review
```

TDD 路径：

```text
Skills /grill-with-docs
Skills /tdd
gstack /review
```

怎么选：

| 任务大小 | 推荐 |
|---|---|
| 改文案、typo、小配置 | `GSD /gsd-fast` |
| 小功能但有边界 | `GSD /gsd-quick --full` |
| 小功能但质量要求高 | `Skills /tdd` |
| 小 UI 改动 | `gstack /design-review` + `/qa` |
| 需求很容易误解 | `Skills /grill-with-docs` |

## 场景 7：UI / 设计 / 前端体验

目标：避免 AI 做出“能跑但丑、乱、不可用”的界面。

从想法到设计：

```text
gstack /design-consultation
gstack /design-shotgun
Skills /prototype
gstack /design-html
```

进入 GSD phase：

```text
GSD /gsd-discuss-phase 1
GSD /gsd-ui-phase 1
GSD /gsd-plan-phase 1
GSD /gsd-execute-phase 1
```

实现后审查：

```text
gstack /design-review
GSD /gsd-ui-review 1
gstack /qa
```

最佳实践：

| 做 | 原因 |
|---|---|
| 先用 `/design-shotgun` 看多个方向 | 视觉偏好很难一次说清 |
| 用 `/prototype` 快速试 UI 变体 | 不把探索代码误当生产实现 |
| 用 `/gsd-ui-phase` 固化 UI contract | 避免实现时偏离 |
| 用 `/design-review` 修实际代码 | 不只停留在建议 |
| 用 `/qa` 真实点击 | UI 问题很多只有浏览器里才会出现 |

## 场景 8：API / CLI / SDK / 开发者产品

目标：确保开发者体验清晰、上手快、错误可理解。

推荐路径：

```text
gstack /plan-devex-review
Skills /grill-with-docs
GSD /gsd-discuss-phase 1 --analyze
Skills /to-prd
Skills /to-issues
GSD /gsd-plan-phase 1
GSD /gsd-execute-phase 1
gstack /devex-review
gstack /document-release
```

加强 TDD：

```text
Skills /tdd
Superpowers requesting-code-review
```

重点检查：

| 项 | 推荐工具 |
|---|---|
| TTHW 是否足够短 | `gstack /plan-devex-review`、`/devex-review` |
| flags、返回格式、错误处理 | `GSD /gsd-discuss-phase` |
| 领域术语和文档一致 | `Skills /grill-with-docs` |
| 文档是否同步 | `gstack /document-release` |
| 代码是否按计划 | `Superpowers requesting-code-review` |

## 场景 9：架构规划 / 技术方案

目标：在写代码前锁定数据流、边界、依赖、测试和失败模式。

推荐路径：

```text
GSD /gsd-map-codebase
Skills /zoom-out
gstack /plan-eng-review
GSD /gsd-discuss-phase 1 --analyze
GSD /gsd-plan-phase 1 --reviews
Skills /to-issues
```

架构保养：

```text
Skills /improve-codebase-architecture
gstack /review
```

如果是安全敏感：

```text
gstack /cso
gstack /plan-eng-review
GSD /gsd-plan-phase 1
```

如果是性能敏感：

```text
gstack /benchmark
Skills /diagnose
gstack /plan-eng-review
GSD /gsd-plan-phase 1
```

## 场景 10：执行开发 / 写代码

目标：让 agent 按计划做小步、可验证、可回滚的实现。

长周期执行：

```text
GSD /gsd-execute-phase 1
```

严格工程纪律：

```text
Superpowers using-git-worktrees
Superpowers subagent-driven-development
Skills /tdd
Superpowers requesting-code-review
```

执行中检查：

```text
gstack /review
GSD /gsd-progress
GSD /gsd-health
Skills /zoom-out
```

最佳实践：

| 做 | 原因 |
|---|---|
| GSD 负责 phase/wave 执行 | 避免主上下文腐烂 |
| Skills `/tdd` 负责 red-green-refactor | 避免先写实现后补假测试 |
| Superpowers 负责 worktree 和过程纪律 | 让执行步骤更可控 |
| gstack 负责跨角色评审 | 找到生产风险、设计问题、安全问题 |

## 场景 11：代码评审 / 第二意见

目标：找出 CI 过了但生产会出问题的缺陷。

推荐路径：

```text
gstack /review
gstack /codex
Superpowers requesting-code-review
GSD /gsd-review
```

如果收到反馈：

```text
Superpowers receiving-code-review
```

如果问题指向架构：

```text
Skills /zoom-out
Skills /improve-codebase-architecture
```

建议顺序：

| 顺序 | 工具 | 目的 |
|---|---|---|
| 1 | `Superpowers requesting-code-review` | 对照计划和 TDD 纪律 |
| 2 | `gstack /review` | 找生产风险和 completeness gaps |
| 3 | `gstack /codex` | 跨模型第二意见 |
| 4 | `GSD /gsd-review` | 回到 GSD 阶段上下文中审查 |
| 5 | `Skills /improve-codebase-architecture` | 如果发现复杂度问题，做架构层修复 |

## 场景 12：QA / 浏览器验证 / UAT

目标：确认功能真的可用，而不是只看测试通过。

推荐路径：

```text
GSD /gsd-verify-work 1
gstack /qa <URL>
gstack /qa-only <URL>
```

需要登录态：

```text
gstack /setup-browser-cookies
gstack /qa <URL>
```

需要你手动处理验证码/MFA：

```text
gstack /open-gstack-browser
$B handoff
$B resume
```

分工：

| 工具 | 负责什么 |
|---|---|
| `GSD /gsd-verify-work` | 按 phase 交付项带你人工验收 |
| `gstack /qa` | 真实浏览器点击、发现并修复 bug |
| `gstack /qa-only` | 只出 QA 报告，不改代码 |
| `Skills /tdd` | 给 QA 发现的问题补回归测试 |
| `Superpowers verification-before-completion` | 宣布完成前要求证据 |

## 场景 13：调试 / 线上故障 / 卡住

目标：停止猜，先找根因。

推荐路径：

```text
Skills /diagnose
gstack /investigate
GSD /gsd-debug [desc]
```

如果工作流本身失败：

```text
GSD /gsd-forensics
```

如果修完要确认：

```text
Skills /tdd
Superpowers verification-before-completion
gstack /qa
```

最佳实践：

| 做 | 不做 |
|---|---|
| reproduce -> minimise -> hypothesise -> instrument -> fix -> regression-test | 一边猜一边改 |
| 保留日志和命令证据 | 只说“应该修好了” |
| 修复后补回归测试 | 只手动点一遍 |

## 场景 14：安全审计

目标：上线前找高风险漏洞，并要求可复现攻击场景。

推荐路径：

```text
gstack /cso
gstack /review
GSD /gsd-plan-phase 1 --reviews
```

高风险开发时加保护：

```text
gstack /guard
gstack /freeze
gstack /careful
Skills /git-guardrails-claude-code
```

安全敏感文件处理：

| 做法 | 目的 |
|---|---|
| deny list 禁止读取 `.env`、keys、pem | 防止 agent 读取 secrets |
| 用 `/cso` 做 OWASP + STRIDE | 找可利用漏洞 |
| 用 `/review` 查代码风险 | 找实现层问题 |
| 用 `/git-guardrails-claude-code` | 阻止危险 git 命令 |

## 场景 15：发布 / PR / 生产验证

目标：把已验证功能安全送到 PR 和生产。

GSD 阶段发布：

```text
GSD /gsd-ship 1
GSD /gsd-ship 1 --draft
```

gstack 发布链：

```text
gstack /ship
gstack /land-and-deploy
gstack /canary
```

PRD/Issue 链：

```text
Skills /to-prd
Skills /to-issues
```

文档同步：

```text
gstack /document-release
gstack /document-generate
```

推荐组合：

```text
GSD /gsd-verify-work 1
gstack /qa
gstack /review
GSD /gsd-ship 1
gstack /ship
gstack /land-and-deploy
gstack /canary
gstack /document-release
```

## 场景 16：并行开发 / 多工作流

目标：多个功能并行推进，但不互相污染。

GSD 路径：

```text
GSD /gsd-workstreams create <name>
GSD /gsd-workstreams switch <name>
GSD /gsd-workspace --new
GSD /gsd-progress --next
```

Superpowers 路径：

```text
Superpowers using-git-worktrees
Superpowers dispatching-parallel-agents
Superpowers subagent-driven-development
```

gstack 路径：

```text
gstack /pair-agent
gstack /browse
```

Skills 路径：

```text
Skills /to-issues
Skills /handoff
```

选择建议：

| 并行类型 | 推荐 |
|---|---|
| 多个里程碑/版本线 | `GSD workstreams` |
| 多个独立代码任务 | `Superpowers dispatching-parallel-agents` |
| 多个 agent 共用浏览器测试 | `gstack /pair-agent` |
| 多个隔离仓库副本 | `GSD /gsd-workspace --new` |
| 多个独立可领取任务 | `Skills /to-issues` |

## 场景 17：暂停、恢复、长期记忆

目标：中断后能接上，项目经验能沉淀。

GSD 会话恢复：

```text
GSD /gsd-pause-work --report
GSD /gsd-resume-work
GSD /gsd-progress
```

Skills 交接：

```text
Skills /handoff
```

gstack 记忆：

```text
gstack /learn
gstack /setup-gbrain
gstack /sync-gbrain
```

Superpowers 完成分支：

```text
Superpowers finishing-a-development-branch
```

推荐做法：

| 场景 | 工具 |
|---|---|
| 临时暂停 | `GSD /gsd-pause-work` |
| 换 agent 或换会话 | `Skills /handoff` |
| 恢复上下文 | `GSD /gsd-resume-work` |
| 长期项目知识 | `gstack /learn`、`/setup-gbrain` |
| 分支收尾 | `Superpowers finishing-a-development-branch` |

## 场景 18：文档和知识沉淀

目标：代码、文档、路线图和学习都保持同步。

推荐路径：

```text
Skills /grill-with-docs
gstack /document-release
gstack /document-generate
GSD /gsd-milestone-summary
gstack /retro
gstack /learn
```

什么时候用：

| 情况 | 推荐 |
|---|---|
| 功能刚发布 | `gstack /document-release` |
| 项目缺少文档 | `gstack /document-generate` |
| 里程碑完成 | `GSD /gsd-milestone-summary` |
| 需要共享语言/ADR | `Skills /grill-with-docs` |
| 每周复盘 | `gstack /retro` |
| 记录项目经验 | `gstack /learn` |

## 场景 19：仓库工程化基础设施

目标：让 repo 更适合 agent 长期协作。

推荐路径：

```text
Skills /setup-matt-pocock-skills
Skills /setup-pre-commit
Skills /git-guardrails-claude-code
```

适用场景：

| 技能 | 什么时候用 |
|---|---|
| `/setup-matt-pocock-skills` | 每个 repo 第一次使用 Matt Pocock skills 前 |
| `/setup-pre-commit` | 缺少提交前 lint、format、type check、tests |
| `/git-guardrails-claude-code` | 想阻止 push、reset hard、clean 等危险 git 命令 |

## 组合边界

### 不建议全量跑的情况

| 情况 | 原因 | 更好做法 |
|---|---|---|
| typo、文案、小配置 | 流程成本高于任务价值 | `GSD /gsd-fast` |
| 一小时内的小功能 | 完整 GSD + gstack + Superpowers 过重 | `Skills /tdd` 或 `GSD /gsd-quick --full` |
| 已经有清晰计划 | 重复 brainstorm 浪费上下文 | `Skills /tdd` 或 `GSD /gsd-plan-phase` |
| 只想要 QA 报告 | 不该让 agent 自动改代码 | `gstack /qa-only` |
| 只想建立共享语言 | 不需要完整 GSD | `Skills /grill-with-docs` |

### 建议多套组合的情况

| 情况 | 组合方式 |
|---|---|
| 新产品核心功能 | `gstack` 找方向，`Skills` 建共享语言，`GSD` 管阶段 |
| 复杂重构 | `GSD` 管上下文，`Skills /improve-codebase-architecture` 找结构，`gstack` 做 review/QA |
| 安全或支付相关 | `gstack /cso` + `Skills /tdd` + `GSD verify` |
| 多人/多 agent 并行 | `GSD workstreams` + `Superpowers worktrees` + `Skills /to-issues` + `gstack pair-agent` |
| bug 难以复现 | `Skills /diagnose` + `gstack /investigate` + `Superpowers verification-before-completion` |

## 最佳实践模板

### 模板 A：重要功能标准路径

```text
# 需求
gstack /office-hours
Skills /grill-with-docs
GSD /gsd-new-project

# 规划
GSD /gsd-discuss-phase 1
gstack /autoplan
GSD /gsd-plan-phase 1
Skills /to-prd
Skills /to-issues

# 执行
Superpowers using-git-worktrees
GSD /gsd-execute-phase 1
Skills /tdd

# 质量
gstack /review
gstack /codex
gstack /qa
GSD /gsd-verify-work 1

# 发布
gstack /ship
gstack /land-and-deploy
gstack /canary
gstack /document-release
```

### 模板 B：已有项目新增功能

```text
GSD /gsd-map-codebase
Skills /grill-with-docs
gstack /office-hours
GSD /gsd-new-project
GSD /gsd-discuss-phase 1
GSD /gsd-plan-phase 1 --reviews
GSD /gsd-execute-phase 1
Skills /tdd
gstack /review
gstack /qa
GSD /gsd-verify-work 1
gstack /ship
```

### 模板 C：快速但可靠的小任务

```text
Skills /grill-with-docs
GSD /gsd-quick --discuss --research --full
Skills /tdd
gstack /review
Superpowers verification-before-completion
```

### 模板 D：严格 TDD 功能开发

```text
Skills /grill-with-docs
Skills /to-issues
Superpowers using-git-worktrees
Skills /tdd
Superpowers requesting-code-review
gstack /qa
Superpowers finishing-a-development-branch
```

### 模板 E：UI 功能

```text
gstack /design-shotgun
Skills /prototype
gstack /design-html
GSD /gsd-ui-phase 1
GSD /gsd-plan-phase 1
GSD /gsd-execute-phase 1
gstack /design-review
GSD /gsd-ui-review 1
gstack /qa
```

### 模板 F：线上 bug 修复

```text
Skills /diagnose
gstack /investigate
GSD /gsd-debug "bug 描述"
Skills /tdd
gstack /qa
Superpowers verification-before-completion
gstack /ship
```

### 模板 G：代码库复杂度治理

```text
Skills /zoom-out
Skills /improve-codebase-architecture
gstack /plan-eng-review
Skills /to-issues
Skills /tdd
gstack /review
```

## 个人推荐默认策略

日常可以这样默认：

| 任务级别 | 默认路径 |
|---|---|
| 5 分钟小改 | `GSD /gsd-fast` |
| 半天内小功能 | `Skills /grill-with-docs` -> `Skills /tdd` -> `gstack /review` |
| 1-3 天功能 | `gstack /office-hours` -> `Skills /grill-with-docs` -> `GSD phase loop` -> `gstack /qa` |
| 核心功能/重构 | `GSD Core` 主导 + `Skills architecture/TDD` + `gstack review/QA/security` |
| 新产品 | `gstack` 找方向 -> `Skills` 建语言 -> `GSD` 建 roadmap -> `GSD` 执行 -> `gstack` 发布 |
| 难 bug | `Skills /diagnose` -> `gstack /investigate` -> `Skills /tdd` |

最重要的原则：

1. 需求不清楚时，不要进开发。
2. 改代码前先建立共享语言，尤其是业务复杂项目。
3. 项目上下文大时，用 GSD 切 phase，别把所有东西塞进一个会话。
4. 写代码时，用 Skills 或 Superpowers 的 TDD 纪律。
5. 上线前，用 gstack 做 review、QA、安全和发布。
6. 完成后，写文档、做 retro、沉淀 learnings，并在必要时 `/handoff`。

