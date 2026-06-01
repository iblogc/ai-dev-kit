# GSD Core 日常使用速查

> 只保留日常使用相关内容，不含安装、卸载、社区、徽章等信息。目标是快速判断：现在处于哪个阶段，应该用哪个 GSD 指令。

## 总流程

```text
理解代码库 -> 初始化/新里程碑 -> 讨论阶段 -> 规划阶段 -> 执行阶段 -> 人工验收 -> 发 PR -> 完成里程碑
```

推荐主线：

1. `/gsd-map-codebase`：已有代码库先做代码库地图。
2. `/gsd-new-project`：新项目初始化，生成项目、需求、路线图和状态文件。
3. `/gsd-discuss-phase 1`：在规划前把实现偏好和灰区问清楚。
4. `/gsd-plan-phase 1`：研究、拆计划、验证计划。
5. `/gsd-execute-phase 1`：按 wave 并行/串行执行计划，并做阶段验证。
6. `/gsd-verify-work 1`：你亲自做 UAT，确认真的可用。
7. `/gsd-ship 1`：从已验证的工作创建 PR。
8. `/gsd-complete-milestone`：所有阶段完成后归档里程碑并打 tag。
9. `/gsd-new-milestone`：开启下一个版本周期。

也可以让 GSD 自动判断下一步：

```text
/gsd-progress --next
```

## 阶段速查

| 阶段 | 主要指令 | 什么时候用 | 产出 |
|---|---|---|---|
| 已有代码库理解 | `/gsd-map-codebase` | brownfield 项目、接手旧项目、准备新增功能前 | 技术栈、架构、约定、风险点分析 |
| 项目初始化 | `/gsd-new-project [--auto]` | 新项目或第一次引入 GSD | `PROJECT.md`、`REQUIREMENTS.md`、`ROADMAP.md`、`STATE.md`、`.planning/research/` |
| 阶段讨论 | `/gsd-discuss-phase [N]` | phase 只有一两句话，需要补实现偏好 | `{phase_num}-CONTEXT.md` |
| 批量讨论 | `/gsd-discuss-phase <n> --batch` | 想一次回答一组问题，加快讨论 | 更快生成阶段上下文 |
| 查看默认假设 | `/gsd-discuss-phase --assumptions [N]` | 想知道 Claude 打算怎么做再决定是否补充 | 待确认实现假设 |
| 阶段规划 | `/gsd-plan-phase [N]` | 已有阶段上下文，准备生成执行计划 | `{phase_num}-RESEARCH.md`、`{phase_num}-{N}-PLAN.md` |
| 阶段执行 | `/gsd-execute-phase <N>` | 计划已通过，开始实现 | 原子提交、`{phase_num}-{N}-SUMMARY.md`、`{phase_num}-VERIFICATION.md` |
| 人工验收 | `/gsd-verify-work [N]` | 自动验证后，确认功能是否真符合预期 | `{phase_num}-UAT.md`、必要时生成修复计划 |
| 发 PR | `/gsd-ship [N] [--draft]` | 阶段已验收，准备提交 review | PR 和自动生成的 PR 描述 |
| 自动推进 | `/gsd-progress --next` | 不确定下一步该做什么 | 自动检测并执行下一个逻辑步骤 |
| 临时任务 | `/gsd-quick [--full] [--discuss] [--research]` | 不需要完整 phase 流程的小任务 | `.planning/quick/.../PLAN.md`、`SUMMARY.md` |
| 琐碎任务 | `/gsd-fast <text>` | 极小改动，完全跳过规划 | 立即执行 |
| 里程碑审计 | `/gsd-audit-milestone` | 完成前检查是否达到定义 | 缺口报告 |
| 修复里程碑缺口 | `/gsd-audit-milestone --fix` | audit 发现缺口，需要创建补充 phase | 新 phase |
| 完成里程碑 | `/gsd-complete-milestone` | 所有 phase 完成并验收 | 归档、release tag |
| 新里程碑 | `/gsd-new-milestone [name]` | 开启下一个版本 | 新一轮需求、路线图、状态 |
| 里程碑摘要 | `/gsd-milestone-summary` | 需要给团队或未来自己快速理解 | 项目概览 |
| 事后调查 | `/gsd-forensics` | 工作流失败、卡住、结果异常 | 失败原因调查 |

## 标准阶段循环

### 1. 已有代码库先映射

```text
/gsd-map-codebase
```

适用场景：

| 情况 | 为什么先做 |
|---|---|
| 已有项目 | 让 GSD 了解技术栈、架构、约定和风险 |
| 要在旧系统加功能 | 后续 `/gsd-new-project` 会问得更聚焦 |
| 代码库较大 | 规划阶段能自动加载现有模式，减少误判 |

### 2. 初始化项目或里程碑

新项目：

```text
/gsd-new-project
/gsd-new-project --auto
```

新版本/新周期：

```text
/gsd-new-milestone
/gsd-new-milestone <name>
```

GSD 会做：

| 步骤 | 说明 |
|---|---|
| 提问 | 问到理解目标、约束、技术偏好、边界情况 |
| 研究 | 可选但推荐，拉起代理研究领域知识 |
| 需求梳理 | 区分 v1、v2、非范围 |
| 路线图 | 创建与需求映射的 phase 规划 |

### 3. 讨论 phase

```text
/gsd-discuss-phase 1
/gsd-discuss-phase 1 --auto
/gsd-discuss-phase 1 --analyze
/gsd-discuss-phase 1 --batch
/gsd-discuss-phase --assumptions 1
```

适用场景：

| 类型 | 会追问什么 |
|---|---|
| 视觉功能 | 布局、信息密度、交互、空状态 |
| API / CLI | 返回格式、flags、错误处理、详细程度 |
| 内容系统 | 结构、语气、深度、流转 |
| 组织型任务 | 分组标准、命名、去重、例外 |

参数：

| 参数 | 用途 |
|---|---|
| `--auto` | 更自动化地推进 |
| `--analyze` | 增加权衡分析 |
| `--batch` | 一次回答一组问题 |
| `--assumptions` | 查看 Claude 打算采用的默认方案 |

### 4. 规划 phase

```text
/gsd-plan-phase 1
/gsd-plan-phase 1 --auto
/gsd-plan-phase 1 --reviews
/gsd-plan-phase 1 --skip-research
/gsd-plan-phase 1 --skip-verify
```

GSD 会做：

| 步骤 | 说明 |
|---|---|
| 研究 | 读取 `CONTEXT.md`，研究实现方式和模式 |
| 制定计划 | 生成 2-3 份原子化 XML 任务计划 |
| 验证计划 | 对照需求检查，必要时迭代 |

参数：

| 参数 | 用途 |
|---|---|
| `--auto` | 自动化推进规划 |
| `--reviews` | 加载代码库审查结果 |
| `--skip-research` | 单次跳过研究代理 |
| `--skip-verify` | 单次跳过计划验证 |

### 5. 执行 phase

```text
/gsd-execute-phase 1
```

执行特点：

| 机制 | 说明 |
|---|---|
| wave 执行 | 独立计划同一 wave 并行，有依赖的后置 |
| 新上下文 | 每个计划在新上下文执行，降低 context rot |
| 原子提交 | 每个任务单独 commit |
| 目标验证 | 检查代码库是否真的交付该 phase |
| 构建测试门 | 合并后自动跑 build/test，按配置或技术栈自动检测 |

默认构建/测试检测顺序：

```text
workflow.build_command -> Xcode(.xcodeproj) -> Makefile -> Justfile -> Cargo -> Go -> Python -> npm
```

Xcode/iOS 项目会自动运行：

```text
xcodebuild build
xcodebuild test
```

### 6. 人工验收

```text
/gsd-verify-work 1
```

GSD 会：

| 步骤 | 说明 |
|---|---|
| 提取交付项 | 列出现在应该能做到什么 |
| 带你逐项验证 | 你回答可以/不可以/哪里不对 |
| 自动诊断失败 | 拉起 debug agent 找根因 |
| 创建修复计划 | 重新运行 `/gsd-execute-phase` 即可执行修复 |

### 7. 发布阶段成果

```text
/gsd-ship 1
/gsd-ship 1 --draft
```

适用场景：

| 指令 | 用途 |
|---|---|
| `/gsd-ship [N]` | 从已验证阶段创建 PR |
| `/gsd-ship [N] --draft` | 创建 draft PR |
| `/gsd-pr-branch` | 创建过滤 `.planning/` 提交的干净 PR 分支 |

### 8. 完成里程碑

```text
/gsd-audit-milestone
/gsd-audit-milestone --fix
/gsd-complete-milestone
/gsd-new-milestone
```

完成逻辑：

| 指令 | 用途 |
|---|---|
| `/gsd-audit-milestone` | 检查是否达到完成定义 |
| `/gsd-audit-milestone --fix` | 根据缺口创建补充 phase |
| `/gsd-complete-milestone` | 归档当前里程碑并打 release tag |
| `/gsd-new-milestone [name]` | 开始下一个版本周期 |

## 快速模式

```text
/gsd-quick
/gsd-quick --discuss
/gsd-quick --research
/gsd-quick --full
/gsd-quick --discuss --research --full
```

适合：

| 情况 | 用法 |
|---|---|
| 小任务但仍想有 GSD 保障 | `/gsd-quick` |
| 小任务前需要补一点上下文 | `/gsd-quick --discuss` |
| 不确定实现方式或库选型 | `/gsd-quick --research` |
| 希望有计划检查和执行后验证 | `/gsd-quick --full` |
| 想要最稳的小任务流程 | `/gsd-quick --discuss --research --full` |

极小任务：

```text
/gsd-fast <text>
```

`/gsd-fast` 会跳过规划，直接处理琐碎任务。

## Workstreams 并行工作流

| 指令 | 用途 |
|---|---|
| `/gsd-workstreams list` | 显示所有工作流及其状态 |
| `/gsd-workstreams create <name>` | 创建命名空间工作流，用于并行里程碑 |
| `/gsd-workstreams switch <name>` | 切换当前活跃工作流 |
| `/gsd-workstreams complete <name>` | 完成并合并工作流 |

配置继承规则：

| 项 | 说明 |
|---|---|
| `GSD_WORKSTREAM` | 设置后先加载根 `.planning/config.json`，再深合并工作流配置 |
| 工作流优先 | 冲突时工作流配置覆盖根配置 |
| 显式 `null` | 工作流配置里的 `null` 会覆盖根值 |

## 多项目工作区

| 指令 | 用途 |
|---|---|
| `/gsd-workspace --new` | 创建隔离工作区，包含仓库副本（worktree 或 clone） |
| `/gsd-workspace --list` | 显示所有 GSD 工作区及其状态 |
| `/gsd-workspace --remove` | 移除工作区并清理 worktree |

## UI 设计

| 指令 | 用途 |
|---|---|
| `/gsd-ui-phase [N]` | 为前端阶段生成 UI 设计合约 `UI-SPEC.md` |
| `/gsd-ui-review [N]` | 对已实现前端代码做 6 维视觉审计 |

推荐搭配：

```text
/gsd-discuss-phase 1
/gsd-ui-phase 1
/gsd-plan-phase 1
/gsd-execute-phase 1
/gsd-ui-review 1
/gsd-verify-work 1
```

## 阶段管理

| 指令 | 用途 |
|---|---|
| `/gsd-phase` | 在路线图末尾追加 phase |
| `/gsd-phase --insert [N]` | 在 phase 之间插入紧急工作 |
| `/gsd-phase --edit [N]` | 就地修改已有 phase 的字段，编号和位置不变 |
| `/gsd-phase --edit [N] --force` | 跳过确认 diff，验证依赖并更新 `STATE.md` |
| `/gsd-phase --remove [N]` | 删除未来 phase，并重编号 |

`/gsd-phase --edit` 可修改 `ROADMAP.md` 中已有阶段的任意字段，不改变编号或位置，并会验证 `depends_on` 引用。

## 代码质量与验证债务

| 指令 | 用途 |
|---|---|
| `/gsd-review` | 对当前阶段或分支进行跨 AI 同行评审 |
| `/gsd-pr-branch` | 创建过滤 `.planning/` 提交的干净 PR 分支 |
| `/gsd-audit-uat` | 审计验证债务，找出缺少 UAT 的阶段 |
| `/gsd-debug [desc]` | 使用持久状态做系统化调试 |
| `/gsd-forensics` | 对失败或卡住的工作流做事后调查 |

## 积压、笔记和自由文本路由

| 指令 | 用途 |
|---|---|
| `/gsd-capture [desc]` | 记录一个待办想法 |
| `/gsd-capture --seed <idea>` | 将想法存入积压停车场，留给未来里程碑 |
| `/gsd-capture --list` | 查看待办列表 |
| `/gsd-note <text>` | 零摩擦想法捕捉：追加、列出或提升为待办 |
| `/gsd-do <text>` | 将自由文本自动路由到正确的 GSD 命令 |

## 会话暂停与恢复

| 指令 | 用途 |
|---|---|
| `/gsd-pause-work` | 中途暂停时创建交接上下文，写入 `HANDOFF.json` |
| `/gsd-resume-work` | 从上一次会话恢复 |
| `/gsd-pause-work --report` | 生成会话摘要，包含已完成工作和结果 |

## 导航和状态

| 指令 | 用途 |
|---|---|
| `/gsd-progress` | 查看我现在在哪、下一步是什么 |
| `/gsd-progress --next` | 自动检测状态并执行下一步 |
| `/gsd-help` | 显示全部命令和使用指南 |
| `/gsd-update` | 更新 GSD，并预览变更日志 |
| `/gsd-health` | 校验 `.planning/` 目录完整性 |
| `/gsd-health --repair` | 校验并自动修复 `.planning/` 目录 |
| `/gsd-stats` | 显示项目统计：阶段、计划、需求、git 指标 |
| `/gsd-profile-user` | 从会话分析生成开发者行为档案 |
| `/gsd-profile-user --questionnaire` | 通过问卷生成开发者行为档案 |
| `/gsd-profile-user --refresh` | 刷新开发者行为档案 |

## 配置与模型

| 指令 | 用途 |
|---|---|
| `/gsd-settings` | 配置模型 profile 和工作流代理 |
| `/gsd-config --profile quality` | 高质量模式：规划 Opus、执行 Opus、验证 Sonnet |
| `/gsd-config --profile balanced` | 默认平衡模式：规划 Opus、执行 Sonnet、验证 Sonnet |
| `/gsd-config --profile budget` | 省钱模式：规划 Sonnet、执行 Sonnet、验证 Haiku |
| `/gsd-config --profile inherit` | 跟随当前运行时模型选择，适合 OpenRouter、本地模型或 OpenCode `/model` |

核心设置保存在：

```text
.planning/config.json
```

核心配置项：

| Setting | Options | Default | 作用 |
|---|---|---|---|
| `mode` | `yolo`, `interactive` | `interactive` | 自动批准，还是每一步确认 |
| `granularity` | `coarse`, `standard`, `fine` | `standard` | phase 切分粒度 |

工作流代理：

| Setting | Default | 作用 |
|---|---|---|
| `workflow.research` | `true` | 每个 phase 规划前先调研领域知识 |
| `workflow.plan_check` | `true` | 执行前验证计划能否达成阶段目标 |
| `workflow.verifier` | `true` | 执行后确认必须交付项是否落地 |
| `workflow.auto_advance` | `false` | 自动串联 discuss -> plan -> execute |
| `workflow.research_before_questions` | `false` | 讨论提问前先研究 |
| `workflow.skip_discuss` | `false` | 自主模式下跳过讨论阶段 |
| `workflow.discuss_mode` | `null` | 讨论阶段行为；`assumptions` 表示使用推断默认值 |
| `workflow.build_command` | 未设置 | 显式指定构建命令，优先于自动检测 |

执行相关：

| Setting | Default | 作用 |
|---|---|---|
| `parallelization.enabled` | `true` | 是否并行执行独立计划 |
| `planning.commit_docs` | `true` | 是否将 `.planning/` 纳入 git 跟踪 |
| `hooks.context_warnings` | `true` | 显示上下文窗口使用量警告 |

评审模型：

| Setting | 用途 |
|---|---|
| `review.models.<cli>` | 为 codex、gemini 等外部评审 CLI 单独选择模型 |

Git 分支策略：

| Setting | Options | Default | 作用 |
|---|---|---|---|
| `git.branching_strategy` | `none`, `phase`, `milestone` | `none` | 分支创建策略 |
| `git.phase_branch_template` | string | `gsd/phase-{phase}-{slug}` | phase 分支模板 |
| `git.milestone_branch_template` | string | `gsd/{milestone}-{slug}` | milestone 分支模板 |

策略说明：

| 策略 | 含义 |
|---|---|
| `none` | 直接提交到当前分支，GSD 默认行为 |
| `phase` | 每个 phase 创建一个分支，phase 完成时合并 |
| `milestone` | 整个里程碑只用一个分支，里程碑完成时合并 |

里程碑完成时，GSD 会提供 squash merge（推荐）或保留历史的 merge 选项。

## 安全提醒

GSD 的代码库映射和分析命令会读取文件来理解项目。敏感文件应加入运行时 deny list，避免任何命令读取。

建议禁止读取：

```text
.env
.env.*
**/secrets/*
**/*credential*
**/*.pem
**/*.key
```

GSD 有防止提交 secrets 的保护，但第一道防线应该是直接禁止读取敏感文件。

## 产物速查

| 文件/目录 | 作用 |
|---|---|
| `PROJECT.md` | 项目愿景，始终加载 |
| `REQUIREMENTS.md` | 带 phase 可追踪性的 v1/v2 范围定义 |
| `ROADMAP.md` | 路线图，记录要做什么和完成情况 |
| `STATE.md` | 决策、阻塞、当前位置，跨会话记忆 |
| `.planning/research/` | 技术栈、功能、架构、坑点研究 |
| `{phase_num}-CONTEXT.md` | 讨论阶段收集的实现偏好 |
| `{phase_num}-RESEARCH.md` | phase 研究结果 |
| `{phase_num}-{N}-PLAN.md` | XML 原子任务计划 |
| `{phase_num}-{N}-SUMMARY.md` | 单个计划执行总结 |
| `{phase_num}-VERIFICATION.md` | 阶段自动验证结果 |
| `{phase_num}-UAT.md` | 人工验收记录 |
| `.planning/quick/.../PLAN.md` | quick 模式计划 |
| `.planning/quick/.../SUMMARY.md` | quick 模式执行总结 |
| `todos/` | 后续想法和任务 |
| `HANDOFF.json` | 暂停工作时的交接上下文 |
| `UI-SPEC.md` | UI 阶段设计合约 |

## 全部 GSD 指令清单

### 核心工作流

```text
/gsd-new-project [--auto]
/gsd-discuss-phase [N] [--auto] [--analyze]
/gsd-discuss-phase <n> --batch
/gsd-discuss-phase --assumptions [N]
/gsd-plan-phase [N] [--auto] [--reviews]
/gsd-plan-phase [N] --skip-research
/gsd-plan-phase [N] --skip-verify
/gsd-execute-phase <N>
/gsd-verify-work [N]
/gsd-ship [N] [--draft]
/gsd-fast <text>
/gsd-progress --next
/gsd-audit-milestone
/gsd-audit-milestone --fix
/gsd-complete-milestone
/gsd-new-milestone [name]
/gsd-milestone-summary
/gsd-forensics
```

### Workstreams

```text
/gsd-workstreams list
/gsd-workstreams create <name>
/gsd-workstreams switch <name>
/gsd-workstreams complete <name>
```

### Workspace

```text
/gsd-workspace --new
/gsd-workspace --list
/gsd-workspace --remove
```

### UI

```text
/gsd-ui-phase [N]
/gsd-ui-review [N]
```

### 导航

```text
/gsd-progress
/gsd-progress --next
/gsd-help
/gsd-update
```

### Brownfield

```text
/gsd-map-codebase
```

### 阶段管理

```text
/gsd-phase
/gsd-phase --insert [N]
/gsd-phase --edit [N]
/gsd-phase --edit [N] --force
/gsd-phase --remove [N]
/gsd-discuss-phase --assumptions [N]
/gsd-audit-milestone --fix
```

### 代码质量

```text
/gsd-review
/gsd-pr-branch
/gsd-audit-uat
/gsd-debug [desc]
```

### 积压

```text
/gsd-capture [desc]
/gsd-capture --seed <idea>
/gsd-capture --list
/gsd-note <text>
/gsd-do <text>
```

### 会话

```text
/gsd-pause-work
/gsd-resume-work
/gsd-pause-work --report
```

### 工具

```text
/gsd-settings
/gsd-config --profile quality
/gsd-config --profile balanced
/gsd-config --profile budget
/gsd-config --profile inherit
/gsd-quick
/gsd-quick --full
/gsd-quick --discuss
/gsd-quick --research
/gsd-quick --discuss --research --full
/gsd-health
/gsd-health --repair
/gsd-stats
/gsd-profile-user
/gsd-profile-user --questionnaire
/gsd-profile-user --refresh
```

### Codex 命令形式

Codex 环境可能使用 `$` 前缀，例如：

```text
$gsd-help
```

同类 GSD 命令在 Codex 中按对应运行时的 skill/命令形式调用。

## 最常用组合

### 新项目从 0 到 PR

```text
/gsd-new-project
/gsd-discuss-phase 1
/gsd-plan-phase 1
/gsd-execute-phase 1
/gsd-verify-work 1
/gsd-ship 1
```

### 已有代码库新增功能

```text
/gsd-map-codebase
/gsd-new-project
/gsd-discuss-phase 1
/gsd-plan-phase 1 --reviews
/gsd-execute-phase 1
/gsd-verify-work 1
/gsd-ship 1
```

### 小任务但要保留质量保障

```text
/gsd-quick --discuss --research --full
```

### 极小琐碎任务

```text
/gsd-fast <text>
```

### UI 阶段

```text
/gsd-discuss-phase 1
/gsd-ui-phase 1
/gsd-plan-phase 1
/gsd-execute-phase 1
/gsd-ui-review 1
/gsd-verify-work 1
```

### 工作中断后恢复

```text
/gsd-pause-work --report
/gsd-resume-work
/gsd-progress
/gsd-progress --next
```

### 里程碑完成

```text
/gsd-audit-milestone
/gsd-audit-milestone --fix
/gsd-complete-milestone
/gsd-new-milestone
```

