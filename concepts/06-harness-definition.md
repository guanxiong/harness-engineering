# 概念 6：Harness 的精确定义与组件清单

> 本概念由 LangChain、HumanLayer、Martin Fowler 三篇文章综合提炼，是对 OpenAI 原文的扩展。

## 定义

> **Agent = Model + Harness**
>
> Harness = 模型之外的一切代码、配置和执行逻辑。

裸模型不是智能体——它接受文本/图片/音频/视频，输出文本。它不能原生维护状态、执行代码、访问实时知识、搭建环境。当 harness 给它状态、工具执行、反馈回路和可执行约束时，它才成为智能体。

## 完整组件清单

### 来自 LangChain

| 组件 | 说明 |
|------|------|
| System Prompts | AGENTS.md、CLAUDE.md |
| Tools & MCP | 扩展智能体能力的工具和协议 |
| Skills | 渐进式加载的知识包 |
| 沙箱基础设施 | 文件系统、浏览器、隔离执行环境 |
| 编排逻辑 | 子智能体生成、handoff、模型路由 |
| Hooks/中间件 | compaction、续接、lint 检查 |

### 来自 HumanLayer（六个配置杠杆）

| # | 杠杆 | 要点 |
|---|------|------|
| 1 | AGENTS.md | ≤60 行，禁止自动生成 |
| 2 | MCP Servers | 信任边界 + 工具数量控制 |
| 3 | Skills | 渐进式披露，按需加载 |
| 4 | Sub-Agents | 上下文防火墙，隔离防 context rot |
| 5 | Hooks | 生命周期脚本，成功静默/失败报错 |
| 6 | Back-Pressure | 测试/构建/类型检查 = 自我验证回路 |

### 来自 Martin Fowler（三层框架）

```
┌─────────────────────────────────────────┐
│        Context Engineering              │  知识库 + 动态上下文
├─────────────────────────────────────────┤
│     Architectural Constraints           │  LLM 审查 + linter + 结构测试
├─────────────────────────────────────────┤
│     Garbage Collection Agents           │  定期扫描 + 修复漂移
└─────────────────────────────────────────┘
```

## Harness 与模型训练的耦合

LangChain 文章的关键发现：

- 模型在 post-training 阶段与特定 harness 共同训练
- 模型可能 **overfit 到特定 harness**，换 harness 后表现暴跌
- Terminal Bench 2.0 数据：**纯 harness 优化**可以把排名从 Top 30 拉到 Top 5
- 推论：最适合你任务的 harness，不一定是模型 post-training 时用的那个

## 与 Harness.io（CI/CD 平台）的关系

两者不是同一个东西，但共享同一个工程哲学：

```
AI Harness Engineering          Harness.io (CI/CD)
约束 AI 智能体的行为              约束代码交付的过程
AGENTS.md + linter + 背压       Pipeline + Policy-as-Code + 门控
目标：可靠的代码生成              目标：可靠的代码部署

共同本质：用确定性约束驾驭不确定性系统
         Backpressure over Prescription
```
