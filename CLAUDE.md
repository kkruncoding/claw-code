# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在本仓库中工作时提供指导。

## 编译、格式检查和测试

所有命令均在 `rust/` 目录下执行：

```bash
cargo build --workspace           # 编译所有 crate
cargo fmt                          # 格式化代码
cargo clippy --workspace            # 代码检查（CI 使用此命令）
cargo test --workspace              # 运行所有测试
cargo test -p rusty-claude-cli --test mock_parity_harness  # 运行指定的集成测试
```

运行单个测试：`cargo test -p <crate> -- <test_name>`

## 架构

Rust 工作空间包含 9 个 crate，位于 `rust/crates/`：

| Crate | 职责 |
|-------|------|
| `api` | Provider 客户端（Anthropic/OpenAI/xAI/DashScope）、SSE 流式响应、认证 |
| `commands` | 斜杠命令注册表、解析、帮助文本渲染 |
| `compat-harness` | 从 TS 源码提取 manifest，用于一致性校验 |
| `mock-anthropic-service` | 确定性 Anthropic 兼容 mock 服务，供 CLI 测试使用 |
| `plugins` | 插件安装/启用/禁用/更新 |
| `runtime` | 核心运行时：会话、配置、权限管理、MCP 生命周期、系统提示词 |
| `rusty-claude-cli` | 主 CLI 二进制 `claw` — REPL、交互命令、流式输出渲染 |
| `telemetry` | 会话追踪事件和用量遥测 |
| `tools` | 内置工具规格和执行（bash、文件读写编辑、grep、glob、agent 等） |

二进制文件名为 `claw`（Windows 下为 `claw.exe`）。默认模型为 `claude-opus-4-6`。

## 配置文件加载顺序

运行时配置按以下顺序加载，后者覆盖前者：
1. `~/.claw.json`
2. `~/.config/claw/settings.json`
3. `<repo>/.claw.json`
4. `<repo>/.claw/settings.json`
5. `<repo>/.claw/settings.local.json`

## 关键文档

- `USAGE.md` — 面向任务的用法指南（认证、会话、配置、Provider）
- `rust/README.md` — crate 结构、CLI 接口、特性和工作空间布局
- `PARITY.md` — Rust 重写与上游的一致性状态
- `ROADMAP.md` — 当前路线图和待处理清理工作

## 协作约定

- `src/` 和 `tests/` 为配套验证面，行为变更时需同时更新。
- 共享默认配置放在 `.claude.json`；机器本地覆盖放在 `.claude/settings.local.json`。
