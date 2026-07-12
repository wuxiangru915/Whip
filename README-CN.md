<div align="center">

# Whip

**面向 AI 编程 Agent 的生产级工程技能集。**

融合 [obra/superpowers](https://github.com/obra/superpowers)、[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) 与 [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) -- 20 个技能，覆盖完整软件开发生命周期，从需求提取到生产级前端设计。

![Skills](https://img.shields.io/badge/skills-20-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)
![Platform](https://img.shields.io/badge/platform-Claude%20Code%20%7C%20Cursor%20%7C%20Gemini%20%7C%20Copilot%20%7C%20Codex-blue)

[English](README.md)

</div>

---

## 为什么是 Whip?

AI 工程社区热衷于给 Agent 套上"缰具"（harness）-- 用各种约束来控制 AI。

但我们问了一个问题：*既然能驱动它向前，为什么只是束缚它?*

**Whip** 给你的是鞭子，让你主动驱动和编排你的技能。不要只是约束 AI 工程 -- 鞭策它行动起来。

---

## 安装

### 方式一: 手动安装（全平台）

```bash
git clone https://github.com/wuxiangru915/Whip.git
cp -r Whip/skills/* <你的-agent-skills-目录>/
```

各平台 skills 目录：

| 平台 | Skills 目录 |
|------|------------|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| Gemini CLI | `~/.gemini/extensions/` |
| Copilot CLI | `~/.copilot/skills/` |
| Codex CLI | `~/.codex/skills/` |

### 方式二: 按需选取单个技能

```bash
git clone https://github.com/wuxiangru915/Whip.git
cp -r Whip/skills/whip-brain <你的-agent-skills-目录>/
cp -r Whip/skills/whip-security <你的-agent-skills-目录>/
```

---

## 项目架构

```
Whip/
|-- skills/
|   |-- whip/             # 入口 -- 如何发现和使用技能
|   |-- whip-brain/       # 实现前的设计探索
|   |-- whip-interview/   # 提取已确认的意图
|   |-- whip-plan/        # 将规格拆解为小粒度任务
|   |-- whip-tdd/         # RED-GREEN-REFACTOR 纪律
|   |-- whip-subagent/    # 并行执行 + 两阶段审查
|   |-- whip-execute/     # 带审查检查点的计划执行
|   |-- whip-dispatch/    # 分发独立的并行任务
|   |-- whip-worktree/    # 通过 git worktree 隔离工作区
|   |-- whip-ui/          # 反"AI slop"前端设计（风格路由、预检、AI 痕迹）
|   |-- whip-verify/      # 声称完成前必须有证据
|   |-- whip-debug/       # 四阶段根因调试
|   |-- whip-adversarial/ # 以证伪为导向的决策审查
|   |-- whip-security/    # OWASP Top 10, SSRF, LLM 安全
|   |-- whip-perf/        # 性能 & Core Web Vitals
|   |-- whip-api/         # 契约优先的 API 设计
|   |-- whip-review/      # 提交前代码审查
|   |-- whip-receive/     # 处理代码审查反馈
|   |-- whip-finish/      # 合并、PR 或清理
|   `-- whip-write-skill/ # 创建和编辑技能
|-- LICENSE
`-- README.md
```

---

## 全部技能

### 定义 -- 明确要构建什么

| 命令 | 使用场景 |
|------|----------|
| `whip-brain` | 在任何创造性工作之前 -- 创建功能、构建组件、添加功能。在实现之前探索用户意图、需求和设计。 |
| `whip-interview` | 当需求不明确时（"帮我建个 X"但没有说清为谁建、为什么建），或在计划出现前发现自己正在填补模糊需求时。 |

### 计划 -- 拆解任务

| 命令 | 使用场景 |
|------|----------|
| `whip-plan` | 当你有规格或多步骤任务的需求、尚未动代码时。产出带文件路径和验收标准的小粒度任务。 |

### 构建 -- 编写代码

| 命令 | 使用场景 |
|------|----------|
| `whip-ui` | 当构建或重新设计前端界面时 -- 落地页、作品集、Web 应用。反"AI slop"设计标准：需求推断、三旋钮（方差/动效/密度）、风格路由、40+ 项预飞检查。 |
| `whip-tdd` | 在实现任何功能或修复 bug 之前、写实现代码之前。强制执行 RED-GREEN-REFACTOR。 |
| `whip-subagent` | 当执行包含独立任务的实现计划时。生成子 Agent 并行执行，带两阶段审查。 |
| `whip-execute` | 当你有已编写好的实现计划，需要在独立会话中带审查检查点执行时。 |
| `whip-dispatch` | 当面对 2 个以上可独立完成、无共享状态或顺序依赖的任务时。 |
| `whip-worktree` | 当开始需要与当前工作区隔离的功能开发时。通过 git worktree 创建隔离工作区。 |

### 验证 -- 证明它能工作

| 命令 | 使用场景 |
|------|----------|
| `whip-verify` | 当即将声称工作已完成、已修复或已通过时。要求运行验证命令并确认输出后才能做出成功声明。 |
| `whip-debug` | 当遇到任何 bug、测试失败或意外行为时。四阶段根因调试：先理解再修复。 |

### 审查 -- 合并前的质量门禁

| 命令 | 使用场景 |
|------|----------|
| `whip-adversarial` | 当正确性比速度更重要时。全新上下文的对抗性审查，针对非平凡决策 -- 偏向证伪而非批准。 |
| `whip-security` | 在提交处理用户输入、认证、数据存储或外部集成的代码之前。OWASP Top 10, SSRF, LLM 安全。 |
| `whip-perf` | 当存在性能需求、分析显示瓶颈、或需要改善 Core Web Vitals / 加载时间时。 |
| `whip-api` | 当设计 REST 端点、模块边界、组件 props 或任何公共接口时。契约优先设计, Hyrum 定律, 错词语义。 |
| `whip-review` | 当完成任务、实现重大功能或合并之前。提交前审查：安全扫描、质量门禁、自动修复。 |
| `whip-receive` | 当收到代码审查反馈、在实施建议之前。要求技术严谨和验证，而非表演性同意。 |

### 交付 -- 自信部署

| 命令 | 使用场景 |
|------|----------|
| `whip-finish` | 当实现完成、所有测试通过、需要决定如何集成工作时。合并、PR 或清理。 |

### 元技能 -- 关于技能的技能

| 命令 | 使用场景 |
|------|----------|
| `whip` | 在任何对话开始时。确立如何发现和使用技能，要求在任何回复之前先调用 Skill 工具。 |
| `whip-write-skill` | 当创建新技能、编辑现有技能或在部署前验证技能时。 |

---

## 流程图

```
用户请求
    |
    v
+---------------------------+
|  whip-interview           |  需求不明确? -> 提取真实意图
+------------+--------------+
             |
             v
+---------------------------+
|  whip-brain               |  设计探索 + 访谈技巧
+------------+--------------+
             |
             v
+---------------------------+
|  whip-plan                |  实现计划
+------------+--------------+
             |
             v
+---------------------------+
|  whip-ui                  |  前端设计方向 + 反 slop 规则
+------------+--------------+
             |
             v
+---------------------------+
|  whip-subagent            |  带 TDD 自动执行任务
+------------+--------------+
             |
             v
+---------------------------+
|  whip-verify              |  声称前先取证
+---------------------------+
|  whip-security            |  提交前门禁
|  whip-perf                |
|  whip-adversarial         |  (高风险决策)
+------------+--------------+
             |
             v
+---------------------------+
|  whip-review              |  代码审查
+------------+--------------+
             |
             v
+---------------------------+
|  whip-finish              |  交付
+---------------------------+
```

---

## 快速参考

```bash
# 开发新功能
"Let's build a user dashboard"
# -> whip-brain -> whip-plan -> whip-subagent

# 修复 bug
"The login form throws 500 on empty email"
# -> whip-debug -> whip-tdd

# 提交前审查
"Review this code before I push"
# -> whip-security + whip-perf + whip-review

# 设计 API
"Design the REST API for our task manager"
# -> whip-api -> whip-plan

# 构建落地页
"Build a SaaS landing page with a minimalist aesthetic"
# -> whip-brain -> whip-ui -> whip-subagent

# 验证想法
"I want to add real-time notifications"
# -> whip-interview -> whip-brain
```

---

## 致谢

融合了三个优秀的开源项目：

- **[obra/superpowers](https://github.com/obra/superpowers)** -- 执行引擎、TDD 纪律、子 Agent 编排
- **[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)** -- 质量门禁、安全模式、API 设计、需求提取
- **[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)** -- 反"AI slop"前端设计、风格体系、预飞检查、AI 痕迹检测

## 许可证

MIT
