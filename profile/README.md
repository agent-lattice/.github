<div align="center">

# ⬡ Agent Lattice

**开源 Agent 配置底座 · 自托管 · 内核可替换**

为不同类型的应用提供 Agent 定制、运行托管和统一协议支撑

[🌐 官网](https://agent-lattice.cn) · [🎮 Demo](https://demo.agent-lattice.cn) · [📖 文档](https://github.com/agent-lattice/agent-lattice/tree/main/docs) · [💬 讨论](https://github.com/agent-lattice/.github/discussions)

</div>

---

## 为什么需要 Agent Lattice？

当你把 Agent 嵌入自己的产品时，会立刻面对这些问题：

> 🤔 **每次换一个 Agent 内核，前端就要重写一遍？**
> 🤔 **Agent 的状态、会话、文件散落各处，无法统一管理？**
> 🤔 **想要自托管，却发现只能用云上的 Managed 服务？**
> 🤔 **多租户隔离、凭证安全、审批流程——从零搭建太痛苦？**

**Agent Lattice 把这些都变成底座能力**——你只需要关注业务逻辑，基础设施由平台接管。

---

## 三平面架构

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│   交互平面  agent-lattice-manager-ui                      │
│   ┌──────────────────────────────────────────┐            │
│   │  AgentShell · ChatPane · Composer        │            │
│   │  ApprovalDialog · ToolTimeline · Files   │            │
│   │         ↕ 统一 Agent UI 协议 ↕           │            │
│   └──────────────────────────────────────────┘            │
│                          │                                │
│                          │ SSE + REST                     │
│                          ▼                                │
│   控制平面  agent-gateway                                │
│   ┌──────────────────────────────────────────┐            │
│   │  会话路由 · 事件转换 · Worker 调度       │            │
│   │  文件读写 · SSE 推送 · 审批代理          │            │
│   │         ↕ RuntimeProvider 合同 ↕         │            │
│   └──────────────────────────────────────────┘            │
│                          │                                │
│                          │ mounts + env + lifecycle       │
│                          ▼                                │
│   执行平面  Worker Provider                               │
│   ┌──────────────────────────────────────────┐            │
│   │  Docker Sandbox · 阿里云 FC · AWS Lambda │            │
│   │  按需拉起 · 执行任务 · 自动回收          │            │
│   └──────────────────────────────────────────┘            │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

**前端消费的是协议，不是执行器。** 更换 Agent 内核，前端零改动。

---

## 四条设计原则

| 原则 | 含义 |
|:-----|:-----|
| **状态落盘，服务轻量** | 会话、历史、工具、审批全部写入用户目录，Gateway 不依赖重数据库，重启即可恢复 |
| **协议统一，内核可替换** | 前端只认 Agent UI 协议（12 种事件类型），后端 Provider 可替换 |
| **运行时容器化，调度集中在网关** | Worker 按需拉起、自动回收，不是常驻服务 |
| **单机先可用，后续可演进多租** | 用户目录隔离 → 容器隔离 → 多租部署，平滑演进 |

---

## 核心能力

<table>
<tr>
<td width="50%">

### 🧩 Agent 模板管理

从 `agent-spec` 模板一键创建用户实例——
`CLAUDE.md`、`settings.json`、`rules/`、`skills/`、`commands/`
自动分发到每个用户的独立目录

</td>
<td width="50%">

### 🔄 会话生命周期

`idle → busy → waiting_approval → archived → stopped`
五种状态完整覆盖，每次执行都有 durable event log

</td>
</tr>
<tr>
<td width="50%">

### 🛡️ 审批交互

Agent 执行高风险操作时自动暂停，
等待人类确认后才继续——
`approval.requested → approval.resolved`

</td>
<td width="50%">

### 📁 文件系统为主真相源

用户项目、Claude Home、运行态投影三层目录结构，
所有状态均可从文件恢复，无需数据库

</td>
</tr>
<tr>
<td width="50%">

### 🌊 SSE 实时事件流

12 种统一事件类型——
`run` / `message` / `tool` / `terminal` / `artifact` / `approval` / `state` / `error`
前端一套订阅逻辑覆盖全部场景

</td>
<td width="50%">

### 🔌 RuntimeProvider 可插拔

Docker · 阿里云 FC · AWS Lambda
三段式 Launch Spec (`mounts + env + lifecycle`)，
新增运行时只需实现一个 Protocol

</td>
</tr>
</table>

---

## 资源模型

```
user ──┬── agent ──┬── session (idle / busy / waiting_approval / archived / stopped)
       │           ├── run       (单轮执行)
       │           └── messages  (多轮上下文执行)
       │
       ├── project ── agent ── session ── run / messages
       │              (与 user 模式同构)
       │
       ├── agent-spec ──→ 用户实例 (模板分发)
       │
       └── WebDAV ──→ 项目文件 / Claude Home / agent-spec
```

---

## 组件一览

| 组件 | 语言 | 定位 |
|:-----|:-----|:-----|
| **agent-gateway** | Python · FastAPI | 控制平面：路由、调度、事件转换、文件读写 |
| **agent-lattice-manager-ui** | TypeScript · React · Next.js | 交互平面：14 个组件 + 5 个 Hooks + 2 个 Providers |
| **claude-agent-sanbox-worker** | Python · Docker | 执行平面：沙箱化 Agent 运行环境 |

**前端组件库**可直接嵌入你的产品：
`AgentShell · HeaderBar · SessionSidebar · ChatPane · MessageList · Composer · ToolTimeline · ToolCallCard · ApprovalDialog · AgentProjectTree · FileViewer · DiffViewer · TerminalPanel · EmptyState`

---

## 快速开始

```bash
# 克隆项目
git clone https://github.com/agent-lattice/agent-lattice.git
cd agent-lattice

# 开发环境一键启动
make dev-up

# 访问
# Gateway API:  http://localhost:8000
# Manager UI:   http://localhost:3177
```

或直接体验在线 Demo：[demo.agent-lattice.cn](https://demo.agent-lattice.cn)

---

## 演进路线

```
Phase 0 ━━▶ 目录与资源模型收敛         ✅ 已落地
Phase 1 ━━▶ Durable Session Log       🚧 设计完成
Phase 2 ━━▶ Harness 外置化            📋 规划中
Phase 3 ━━▶ Sandbox 标准化            📋 规划中
Phase 4 ━━▶ 凭证 Broker / Vault       📋 规划中
Phase 5 ━━▶ Session Replay & Recovery 📋 规划中
Phase 6 ━━▶ Many Brains / Many Hands  📋 规划中
```

---

## 与 Managed Claude CLI 的对比

| | Managed Claude CLI | Agent Lattice |
|:--|:------------------|:--------------|
| **部署** | 云托管 | 自托管 |
| **数据归属** | 云端存储 | 本地文件系统，完全可控 |
| **内核** | Claude 专属 | 可替换任何 Agent Provider |
| **前端** | 官方 CLI | 可嵌入组件库，定制你的 UI |
| **运行时** | 固定 | Docker / Serverless / 自定义 |
| **审批流** | 内置 | 可定制，通过 SSE 实时推送 |
| **开源** | ❌ | ✅ MIT |

---

## 技术栈

```
后端   Python 3.12+ · FastAPI · Pydantic · SSE
前端   React 19 · Next.js 15 · TypeScript 5.8 · shadcn/ui · Tailwind 4
构建   Turborepo · pnpm Workspace
容器   Docker · Docker Compose
存储   文件系统（可扩展至数据库）
部署   Docker · 阿里云 FC · AWS Lambda
```

---

## 贡献

我们欢迎各种形式的贡献！

- **Bug / Feature** → [Issues](https://github.com/agent-lattice/.github/issues)
- **想法 / 反馈** → [Discussions](https://github.com/agent-lattice/.github/discussions)
- **代码** → Fork → Branch → PR
- **文档** → 帮助改进 `docs/` 下的设计文档

---

<div align="center">

**⬡ Agent Lattice** — Agent 的底座，不是 Agent 本身

让 Agent 嵌入你的产品，而不是让你的产品围绕某个 Agent

[MIT License](./LICENSE) · Made with ❤️ by the community

</div>