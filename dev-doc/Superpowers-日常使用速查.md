# Superpowers 日常使用速查

> 只保留日常使用相关内容，不含安装步骤。目标是快速判断：现在处于哪个阶段，应该触发/使用哪个 Superpowers 技能。

## 总流程

```text
澄清想法 -> 确认设计 -> 建隔离分支 -> 写计划 -> TDD 实现 -> 评审 -> 完成分支
```

推荐主线：

1. `brainstorming`：写代码前先问清楚真正要做什么。
2. `using-git-worktrees`：设计确认后创建隔离 worktree 和新分支。
3. `writing-plans`：把设计拆成小任务和可验证步骤。
4. `subagent-driven-development` 或 `executing-plans`：按计划执行。
5. `test-driven-development`：实现阶段强制 RED-GREEN-REFACTOR。
6. `requesting-code-review`：任务之间做代码评审。
7. `finishing-a-development-branch`：完成后测试、选择 merge/PR/保留/丢弃，并清理 worktree。

Superpowers 的核心规则：agent 在任何任务前都应该检查相关技能；这些是强制工作流，不是建议。

## 阶段速查

| 阶段 | 主要技能 | 什么时候用 | 产出 |
|---|---|---|---|
| 想法澄清 | `brainstorming` | 有粗略想法、还没写代码前 | 追问、替代方案、分段展示的设计、设计文档 |
| 设计批准后建环境 | `using-git-worktrees` | 设计已确认，需要开始实现 | 新 branch、隔离 worktree、项目 setup、干净测试基线 |
| 写实现计划 | `writing-plans` | 已有批准设计，需要交给 agent 执行 | 2-5 分钟粒度任务、文件路径、代码要求、验证步骤 |
| 子代理开发 | `subagent-driven-development` | 有计划后，希望 agent 自主推进多个任务 | 每个任务一个 fresh subagent、规格符合性评审、代码质量评审 |
| 分批执行计划 | `executing-plans` | 有计划后，希望分批推进并保留人工 checkpoint | 批量执行结果、人类确认点 |
| 测试驱动实现 | `test-driven-development` | 任意实现阶段 | 先红再绿再重构、最小实现、提交 |
| 任务间代码评审 | `requesting-code-review` | 每个任务或批次完成后 | 严重程度排序的问题、阻塞项 |
| 接收评审反馈 | `receiving-code-review` | 收到评审意见后 | 分类处理反馈、修复或解释 |
| 完成开发分支 | `finishing-a-development-branch` | 所有任务完成后 | 测试验证、merge/PR/保留/丢弃选项、worktree 清理 |
| 系统化调试 | `systematic-debugging` | bug 原因不明时 | 4 阶段根因分析、证据链、修复方向 |
| 完成前验证 | `verification-before-completion` | 准备宣布完成前 | 确认问题真的修好，而不是只看起来修好 |
| 并行代理调度 | `dispatching-parallel-agents` | 多个独立任务可以并发时 | 并发 subagent 工作流 |
| 创建/修改技能 | `writing-skills` | 需要新增或改进 Superpowers 技能时 | 符合最佳实践并经过测试的技能 |
| 使用系统入门 | `using-superpowers` | 需要了解 Superpowers 技能系统本身时 | 技能系统使用说明 |

## 标准开发工作流

### 1. 先澄清，不直接写代码

使用：

```text
brainstorming
```

适用场景：

| 情况 | 应该做什么 |
|---|---|
| 用户说“帮我做一个功能” | 先追问真实目标、约束、成功标准 |
| 用户描述了痛点但没有方案 | 探索替代方案 |
| 用户已经给了方案但需求仍模糊 | 把方案拆成可确认的设计块 |
| 用户确认设计 | 保存设计文档，进入计划阶段 |

核心要求：

| 要点 | 说明 |
|---|---|
| Socratic design refinement | 通过问题帮助用户想清楚 |
| 分段展示 | 设计要短到用户能读完并确认 |
| 不急着编码 | 在设计确认前不进入实现 |

### 2. 设计通过后创建隔离工作区

使用：

```text
using-git-worktrees
```

适用场景：

| 情况 | 应该做什么 |
|---|---|
| 设计已经批准 | 创建新的 branch 和 worktree |
| 多个功能并行 | 每个功能独立工作区 |
| 开始实现前 | 运行项目 setup，确认测试基线是干净的 |

产出：

| 产出 | 说明 |
|---|---|
| isolated workspace | 避免污染当前工作区 |
| new branch | 每个任务独立分支 |
| clean test baseline | 开工前测试必须是可解释状态 |

### 3. 写给“缺少上下文的新手工程师”也能执行的计划

使用：

```text
writing-plans
```

计划要求：

| 要求 | 说明 |
|---|---|
| 任务粒度小 | 每个任务 2-5 分钟 |
| 文件路径明确 | 指到具体文件 |
| 代码要求完整 | 不能只写抽象描述 |
| 验证步骤明确 | 每个任务知道怎么确认完成 |
| 强调 TDD | 先测试，后实现 |
| 强调 YAGNI | 不构建暂时用不到的东西 |
| 强调 DRY | 减少重复 |

计划应该清楚到：一个热情但缺乏判断力、没有项目上下文、不爱测试的新手工程师也能照着做。

### 4. 执行计划

二选一：

| 技能 | 适合场景 | 特点 |
|---|---|---|
| `subagent-driven-development` | 希望尽量自动化、每个任务独立推进 | 每个任务新 subagent，带两阶段评审 |
| `executing-plans` | 希望分批推进并定期人工确认 | batch execution + checkpoints |

`subagent-driven-development` 的两阶段评审：

| 阶段 | 检查什么 |
|---|---|
| spec compliance review | 是否符合计划和规格 |
| code quality review | 代码质量、可维护性、测试、简单性 |

### 5. 实现时强制 TDD

使用：

```text
test-driven-development
```

RED-GREEN-REFACTOR：

| 阶段 | 动作 |
|---|---|
| RED | 先写失败测试，并确认它真的失败 |
| GREEN | 写最少代码让测试通过 |
| REFACTOR | 在测试保护下清理实现 |
| COMMIT | 每个小步通过后提交 |

硬规则：

| 规则 | 说明 |
|---|---|
| Tests first | 总是先写测试 |
| Watch it fail | 必须看到测试失败，确认测试有效 |
| Minimal code | 只写让测试通过的最小代码 |
| Watch it pass | 必须看到测试通过 |
| Delete pre-test code | 如果先写了实现再写测试，应删除并按 TDD 重来 |

### 6. 任务之间做评审

使用：

```text
requesting-code-review
```

评审关注：

| 类型 | 说明 |
|---|---|
| plan compliance | 是否按计划实现 |
| test quality | 测试是否真实覆盖行为 |
| production risk | 是否有会在生产爆炸的问题 |
| simplicity | 是否过度设计 |
| severity | 按严重程度报告问题 |

处理评审反馈：

```text
receiving-code-review
```

规则：

| 情况 | 处理 |
|---|---|
| Critical issue | 阻塞继续推进，必须先处理 |
| 明确 bug | 修复并验证 |
| 设计争议 | 给出证据和权衡 |
| 无效反馈 | 说明为什么不采纳 |

### 7. 完成开发分支

使用：

```text
finishing-a-development-branch
```

完成前检查：

| 检查 | 说明 |
|---|---|
| 全量测试 | 确认测试通过 |
| 功能验证 | 确认需求真的完成 |
| 分支状态 | 查看改动、提交、未跟踪文件 |
| 选择下一步 | merge、开 PR、保留分支、丢弃分支 |
| 清理 worktree | 不留下多余工作区 |

## 调试工作流

### 根因不明时

使用：

```text
systematic-debugging
```

适用场景：

| 情况 | 应该做什么 |
|---|---|
| bug 原因不清楚 | 不猜测，先调查 |
| 修了几次还复现 | 回到系统化调试 |
| 有 race condition、异步、状态问题 | 建立证据链 |
| 问题只在某些环境出现 | 追踪条件差异 |

相关方法：

| 方法 | 用途 |
|---|---|
| `root-cause-tracing` | 从症状追到根因 |
| `defense-in-depth` | 设计多层防护，避免单点假设 |
| `condition-based-waiting` | 用条件等待替代固定 sleep |

### 宣布完成前

使用：

```text
verification-before-completion
```

检查重点：

| 检查 | 说明 |
|---|---|
| 复现路径 | 原 bug 的复现路径是否真的通过 |
| 测试 | 是否有自动化测试保护 |
| 证据 | 是否有命令输出、截图、日志等证据 |
| 不只看表象 | 不因为“看起来没报错”就宣布完成 |

## 并行开发

使用：

```text
dispatching-parallel-agents
```

适用场景：

| 情况 | 是否适合 |
|---|---|
| 多个任务互不依赖 | 适合并行 |
| 多个文件区域独立 | 适合并行 |
| 共享同一状态或同一文件 | 谨慎，容易冲突 |
| 需要统一设计判断 | 先 `brainstorming` 和 `writing-plans` |

配套技能：

```text
using-git-worktrees
subagent-driven-development
requesting-code-review
```

## 按场景选择技能

| 你现在要做什么 | 用这个 |
|---|---|
| 我有个想法，不确定怎么做 | `brainstorming` |
| 需求已经确认，准备开工 | `using-git-worktrees` |
| 我要把设计拆成任务 | `writing-plans` |
| 我要 agent 自主按任务推进 | `subagent-driven-development` |
| 我要分批执行并定期确认 | `executing-plans` |
| 我要实现功能 | `test-driven-development` |
| 我要在任务间检查代码 | `requesting-code-review` |
| 我要处理评审意见 | `receiving-code-review` |
| 我要完成这个分支 | `finishing-a-development-branch` |
| 我要查 bug 根因 | `systematic-debugging` |
| 我要确认问题真的修好了 | `verification-before-completion` |
| 我要并发跑多个独立任务 | `dispatching-parallel-agents` |
| 我要新增或修改技能 | `writing-skills` |
| 我要了解 Superpowers 本身 | `using-superpowers` |

## 核心原则

| 原则 | 含义 |
|---|---|
| Test-Driven Development | 永远测试先行 |
| Systematic over ad-hoc | 用系统流程，不靠猜 |
| Complexity reduction | 简单性是首要目标 |
| Evidence over claims | 用证据证明完成，不靠口头声明 |
| YAGNI | 不做暂时用不到的东西 |
| DRY | 避免重复实现 |

## 全部技能清单

### Testing

```text
test-driven-development
```

### Debugging

```text
systematic-debugging
verification-before-completion
root-cause-tracing
defense-in-depth
condition-based-waiting
```

### Collaboration

```text
brainstorming
writing-plans
executing-plans
dispatching-parallel-agents
requesting-code-review
receiving-code-review
using-git-worktrees
finishing-a-development-branch
subagent-driven-development
```

### Meta

```text
writing-skills
using-superpowers
```

## 最常用组合

### 从想法到可合并分支

```text
brainstorming
using-git-worktrees
writing-plans
subagent-driven-development
test-driven-development
requesting-code-review
finishing-a-development-branch
```

### 想更稳、更可控地执行

```text
brainstorming
writing-plans
executing-plans
requesting-code-review
verification-before-completion
finishing-a-development-branch
```

### 修 bug

```text
systematic-debugging
test-driven-development
verification-before-completion
requesting-code-review
```

### 多任务并行开发

```text
brainstorming
writing-plans
using-git-worktrees
dispatching-parallel-agents
subagent-driven-development
requesting-code-review
finishing-a-development-branch
```

### 修改或创建技能

```text
writing-skills
test-driven-development
verification-before-completion
requesting-code-review
```

