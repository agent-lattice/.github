# Agent Lattice

Managed Claude CLI 的开源替代方案

## 项目简介

Agent Lattice 是一个开源的 Claude CLI 管理平台,提供自托管的 Agent 管理能力,作为 Managed Claude CLI 的开源替代品。

## 核心特性

- 🔧 **自托管控制** - 完全掌控你的 Agent 运行环境
- 🐳 **容器化部署** - 基于 Docker 的轻量级部署方案
- 🔄 **灵活扩展** - 模块化设计,易于扩展和定制
- 📊 **可视化管理** - 提供直观的 Agent 状态监控界面

## 架构概览

```text
┌─────────────────────────────────────┐
│         Agent Gateway (API)         │
│    用户/项目管理 / 会话调度          │
└──────────────┬──────────────────────┘
               │
    ┌──────────┴──────────┐
    │                     │
┌───▼────┐          ┌────▼───┐
│ Worker │          │  UI    │
│ Provider│          │ Manager│
└────────┘          └────────┘
```

## 项目状态

🚧 **项目处于早期开发阶段** 🚧

本项目刚刚启动,核心功能正在积极开发中。欢迎社区贡献和反馈!

## 设计目标

### 核心组件

- **agent-gateway** - FastAPI 后端服务
  - 用户/项目目录管理
  - 会话状态管理
  - Worker 调度
  - 标准 Agent UI 协议

- **agent-lattice-manager-ui** - React/Next.js 前端
  - 可复用组件库
  - Agent 管理界面

- **claude-agent-sandbox-worker** - Worker 提供者
  - 沙箱化的 Claude Agent 运行环境
  - 通过 Provider 接口与 Gateway 通信

### 架构理念

```text
┌─────────────────────────────────────┐
│         Agent Gateway (API)         │
│    用户/项目管理 / 会话调度          │
└──────────────┬──────────────────────┘
               │
    ┌──────────┴──────────┐
    │                     │
┌───▼────┐          ┌────▼───┐
│ Worker │          │  UI    │
│ Provider│          │ Manager│
└────────┘          └────────┘
```

## 文档

(待完善)
- 架构设计文档
- 实现路线图
- API 规范

## 与 Managed Claude CLI 的对比

| 特性 | Managed Claude CLI | Agent Lattice |
|------|-------------------|---------------|
| 部署方式 | 托管服务 | 自托管 |
| 数据控制 | 云端存储 | 本地存储 |
| 定制能力 | 有限 | 完全可定制 |
| 成本 | 订阅费用 | 自担基础设施成本 |
| 开源 | ❌ | ✅ |

## 技术栈

- **后端**: FastAPI, Python
- **前端**: React, Next.js, TypeScript
- **容器化**: Docker, Docker Compose
- **存储**: 文件系统 (可扩展至数据库)

## 开发路线图

- [ ] 项目初始化与基础结构
  - [ ] 目录结构设计
  - [ ] 基础配置文件
  - [ ] CI/CD 流程
- [ ] 核心组件开发
  - [ ] Agent Gateway 框架
  - [ ] Worker Provider 接口
  - [ ] Manager UI 基础界面
- [ ] 功能完善
  - [ ] 多租户支持
  - [ ] 监控告警集成
  - [ ] 插件系统
- [ ] 生产就绪
  - [ ] 安全加固
  - [ ] 性能优化
  - [ ] 完整文档

## 贡献

我们欢迎各种形式的贡献!

### 如何参与

1. **报告问题** - 在 Issues 中提交 Bug 报告或功能建议
2. **参与讨论** - 在 Discussions 中分享想法和反馈
3. **提交代码** - Fork → Branch → PR 的标准流程
4. **完善文档** - 帮助改进文档和示例

### 开发指南

(待补充)

## 相关资源

- [Managed Claude CLI 官方文档](https://docs.anthropic.com/claude/docs/claude-cli) - 了解原始服务
- [Claude API 文档](https://docs.anthropic.com/claude/reference) - API 参考

## 许可证

[MIT License](./LICENSE)

## 致谢

本项目受到 Managed Claude CLI 的启发,旨在为社区提供一个开源、可自托管的替代方案。

## 联系方式

- Issues: [GitHub Issues](https://github.com/agent-lattice/.github/issues)
- Discussions: [GitHub Discussions](https://github.com/agent-lattice/.github/discussions)