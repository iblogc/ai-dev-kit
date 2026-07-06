# [mattpocock/skills](https://github.com/mattpocock/skills) 日常使用速查

> 只保留日常使用相关内容，不含安装、newsletter、徽章等信息。目标是快速判断：现在处于哪个工程场景，应该用哪个 Matt Pocock skill。

## 一句话定位

这套 skills 的定位不是接管完整流程，而是提供一组小而可组合的工程基本功：

```text
对齐需求 -> 建立共享语言 -> 写 PRD/Issue -> TDD 实现 -> 诊断问题 -> 改善架构 -> 交接沉淀
```

它适合你想保留控制权，不想让 GSD、BMAD、Spec-Kit 这类完整流程系统接管全部过程时使用。

## 推荐主线

1. `/setup-matt-pocock-skills`：每个 repo 先配置一次 issue tracker、triage labels、文档保存位置。
2. `/grill-with-docs`：任何代码改动前先做追问，并更新 `CONTEXT.md` / ADR。
3. `/to-prd`：把已经讨论清楚的内容整理成 PRD，并提交为 issue。
4. `/to-issues`：把 PRD、spec 或计划拆成可独立领取的 vertical slice issues。
5. `/tdd`：按 red-green-refactor 实现功能或修 bug。
6. `/diagnose`：遇到难 bug 或性能问题时系统化诊断。
7. `/zoom-out`：不理解某段代码时，让 agent 从系统层面解释。
8. `/improve-codebase-architecture`：每隔几天检查一次代码库架构深度和复杂度。
9. `/handoff`：中断或换 agent 前生成交接文档。

## 场景速查

| 场景 | 主要技能 | 什么时候用 | 产出 |
|---|---|---|---|
| 仓库初始配置 | `/setup-matt-pocock-skills` | 每个 repo 第一次使用这些 skills 前 | issue tracker、triage labels、文档布局配置 |
| 非代码需求追问 | `/grill-me` | 文档、计划、设计、决策等非代码任务前 | 完整决策树、澄清后的计划 |
| 代码改动前追问 | `/grill-with-docs` | 每次准备改代码前 | 对齐后的需求、共享语言、`CONTEXT.md`、ADR |
| 生成 PRD | `/to-prd` | 当前对话已经足够清楚，需要沉淀为产品需求 | PRD，并提交为 GitHub issue |
| 拆 issue | `/to-issues` | 有 plan、spec 或 PRD，需要拆成可执行任务 | 多个可独立领取的 GitHub issues |
| TDD 实现 | `/tdd` | 写功能或修 bug | red-green-refactor 循环、垂直切片实现 |
| 诊断 bug | `/diagnose` | 难 bug、性能回退、原因不明 | 复现、最小化、假设、插桩、修复、回归测试 |
| issue 分诊 | `/triage` | 处理 issue backlog | 按 triage roles 的状态机分流 |
| 理解代码 | `/zoom-out` | 不熟悉某段代码或模块 | 系统级上下文、高层解释 |
| 改善架构 | `/improve-codebase-architecture` | 代码库开始变复杂，或例行架构保养 | deep module 机会、架构改进建议 |
| 原型探索 | `/prototype` | 设计还不确定，需要先试 | 可丢弃终端原型或多 UI 变体 |
| 精简沟通 | `/caveman` | 想显著减少 agent 输出 token | 超压缩但技术准确的沟通模式 |
| 会话交接 | `/handoff` | 换 agent、换会话、中断工作 | 可交接上下文文档 |
| 写新 skill | `/write-a-skill` | 想把自己的工作流固化成 skill | 结构正确、渐进披露、带资源的 skill |
| Git 防护 | `/git-guardrails-claude-code` | Claude Code 中想阻止危险 git 命令 | hooks，拦截 push、reset hard、clean 等 |
| 测试迁移 | `/migrate-to-shoehorn` | TypeScript 测试从 `as` 断言迁移 | 使用 `@total-typescript/shoehorn` 的测试文件 |
| 练习脚手架 | `/scaffold-exercises` | 创建课程/练习结构 | sections、problems、solutions、explainers |
| pre-commit | `/setup-pre-commit` | 想统一提交前检查 | Husky、lint-staged、Prettier、type check、tests |

## 核心工作流

### 1. 每个 repo 先配置一次

```text
/setup-matt-pocock-skills
```

它会询问：

| 配置 | 用途 |
|---|---|
| issue tracker | GitHub、Linear 或本地文件 |
| triage labels | `/triage` 分诊 issue 时使用 |
| docs 保存位置 | `/grill-with-docs`、ADR、PRD 等文档输出位置 |

建议在使用这些技能前先跑一次：

```text
/to-issues
/to-prd
/triage
/diagnose
/tdd
/improve-codebase-architecture
/zoom-out
```

### 2. 改代码前先 grill

代码任务：

```text
/grill-with-docs
```

非代码任务：

```text
/grill-me
```

`/grill-with-docs` 的价值：

| 能力 | 说明 |
|---|---|
| 对齐需求 | 通过细问减少“agent 没理解我”的风险 |
| 挑战计划 | 根据现有 domain model 检查你的方案 |
| 建立共享语言 | 把项目术语写入 `CONTEXT.md` |
| 记录决策 | 把难解释的设计决策写进 ADR |
| 降低冗长 | agent 之后能用项目内部术语，而不是每次长篇解释 |

### 3. 把讨论沉淀为 PRD 和 issue

```text
/to-prd
/to-issues
```

区别：

| 技能 | 用途 |
|---|---|
| `/to-prd` | 把当前对话上下文综合成 PRD，并提交为 GitHub issue |
| `/to-issues` | 把任意 plan、spec 或 PRD 拆成可独立领取的 vertical slice issues |

最佳实践：

| 做 | 不做 |
|---|---|
| 先 `/grill-with-docs` 再 `/to-prd` | 需求没问清就直接写 PRD |
| 按 vertical slice 拆 issue | 按技术层拆成 model/API/UI 三个孤立任务 |
| 让每个 issue 可独立验证 | 一堆互相阻塞的半成品 issue |

### 4. 用 TDD 实现

```text
/tdd
```

循环：

```text
RED -> GREEN -> REFACTOR
```

要求：

| 阶段 | 动作 |
|---|---|
| RED | 先写失败测试，并确认失败 |
| GREEN | 写最少代码让测试通过 |
| REFACTOR | 在测试保护下整理代码 |
| 垂直切片 | 每次完成一个可运行的用户价值切片 |

这适合：

| 场景 | 说明 |
|---|---|
| 新功能 | 先写行为测试，再实现 |
| bug fix | 先写复现 bug 的失败测试，再修复 |
| 重构 | 先补保护测试，再改结构 |

### 5. 诊断问题

```text
/diagnose
```

诊断循环：

```text
reproduce -> minimise -> hypothesise -> instrument -> fix -> regression-test
```

适合：

| 问题类型 | 说明 |
|---|---|
| hard bugs | 原因不明或多次修复失败 |
| performance regressions | 性能回退 |
| flaky tests | 不稳定测试 |
| production-only issues | 只在特定环境出现的问题 |

### 6. 理解和改善架构

理解陌生代码：

```text
/zoom-out
```

改善代码库架构：

```text
/improve-codebase-architecture
```

建议频率：

```text
每几天在活跃代码库上跑一次 /improve-codebase-architecture
```

关注点：

| 技能 | 关注什么 |
|---|---|
| `/zoom-out` | 这段代码在整个系统中扮演什么角色 |
| `/improve-codebase-architecture` | 是否有 deep module 机会、是否能降低复杂度、是否符合 `CONTEXT.md` 和 ADR |

### 7. 原型探索

```text
/prototype
```

两种典型产物：

| 类型 | 适合问题 |
|---|---|
| runnable terminal app | 状态流、业务逻辑、算法、流程问题 |
| multiple UI variations | UI 方向不确定，需要多个可切换方案 |

原则：

```text
原型是一次性探索，不应直接当生产实现。
```

## Productivity 技能

| 技能 | 用途 |
|---|---|
| `/caveman` | 超压缩沟通模式，减少约 75% token，同时保留技术准确性 |
| `/grill-me` | 非代码场景下反复追问计划或设计，直到决策树清楚 |
| `/handoff` | 把当前对话压缩成交接文档，让另一个 agent 继续 |
| `/write-a-skill` | 创建新 skill，包含结构、渐进披露和资源打包 |

## Misc 技能

| 技能 | 用途 |
|---|---|
| `/git-guardrails-claude-code` | 配置 Claude Code hooks，阻止危险 git 命令 |
| `/migrate-to-shoehorn` | 把测试文件从 `as` 类型断言迁移到 `@total-typescript/shoehorn` |
| `/scaffold-exercises` | 创建练习目录结构，包含 sections、problems、solutions、explainers |
| `/setup-pre-commit` | 配置 Husky pre-commit hooks、lint-staged、Prettier、type checking 和 tests |

## 共享语言与文档

`/grill-with-docs` 会帮助建立共享语言，通常会更新：

```text
CONTEXT.md
docs/adr/
```

共享语言的收益：

| 收益 | 说明 |
|---|---|
| 更少废话 | 用一个项目术语替代一长段解释 |
| 命名一致 | 变量、函数、文件名更贴近领域语言 |
| agent 更容易导航 | 代码库结构和术语一致 |
| token 更省 | agent 不需要每次重新推理概念 |

示例：

| Before | After |
|---|---|
| `There's a problem when a lesson inside a section of a course is made real` | `There's a problem with the materialization cascade` |

## 全部技能清单

### Engineering

```text
/diagnose
/grill-with-docs
/triage
/improve-codebase-architecture
/setup-matt-pocock-skills
/tdd
/to-issues
/to-prd
/zoom-out
/prototype
```

### Productivity

```text
/caveman
/grill-me
/handoff
/write-a-skill
```

### Misc

```text
/git-guardrails-claude-code
/migrate-to-shoehorn
/scaffold-exercises
/setup-pre-commit
```

## 最常用组合

### 新功能：从想法到 issue

```text
/grill-with-docs
/to-prd
/to-issues
```

### 新功能：从 issue 到代码

```text
/tdd
/diagnose
/zoom-out
```

### bug 修复

```text
/diagnose
/tdd
```

### 代码库变复杂

```text
/zoom-out
/improve-codebase-architecture
/tdd
```

### 换会话或换 agent

```text
/handoff
```

### 精简输出

```text
/caveman
```

### 仓库工程化基础设施

```text
/setup-matt-pocock-skills
/setup-pre-commit
/git-guardrails-claude-code
```

