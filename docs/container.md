# 以容器为优先的 claw-code 工作流

在添加本文档之前，这个仓库的 Rust 运行时已经具备了**容器检测**能力：

- `rust/crates/runtime/src/sandbox.rs` 会检测 Docker/Podman/容器相关标记，例如 `/.dockerenv`、`/run/.containerenv`、匹配的环境变量，以及 `/proc/1/cgroup` 中的线索。
- `rust/crates/rusty-claude-cli/src/main.rs` 会通过 `claw sandbox` / `cargo run -p rusty-claude-cli -- sandbox` 报告暴露该状态。
- `.github/workflows/rust-ci.yml` 在 `ubuntu-latest` 上运行，但**没有**定义 Docker 或 Podman 容器任务。
- 在这次变更之前，仓库中**没有**纳入版本控制的 `Dockerfile`、`Containerfile` 或 `.devcontainer/` 配置。

本文档新增了一个小型、纳入版本控制的 `Containerfile`，让 Docker 和 Podman 用户都有一套统一的容器工作流可用。

## 仓库内置容器镜像的用途

根目录下的 [`../Containerfile`](../Containerfile) 提供了一个可复用的 Rust 构建/测试环境，并预装了该工作区常用的额外软件包（`git`、`pkg-config`、`libssl-dev`、证书）。

它**不会**把仓库内容复制进镜像。相反，推荐的方式是将你的检出目录以 bind mount 的形式挂载到 `/workspace`，这样编辑仍然保留在宿主机上。

## 构建镜像

在仓库根目录执行：

### Docker

```bash
docker build -t claw-code-dev -f Containerfile .
```

### Podman

```bash
podman build -t claw-code-dev -f Containerfile .
```

## 在容器中运行 `cargo test --workspace`

下面这些命令会挂载仓库、避免 Cargo 构建产物落到工作树中，并从位于 `rust/` 的 Rust 工作区运行。

### Docker

```bash
docker run --rm -it \
  -v "$PWD":/workspace \
  -e CARGO_TARGET_DIR=/tmp/claw-target \
  -w /workspace/rust \
  claw-code-dev \
  cargo test --workspace
```

### Podman

```bash
podman run --rm -it \
  -v "$PWD":/workspace:Z \
  -e CARGO_TARGET_DIR=/tmp/claw-target \
  -w /workspace/rust \
  claw-code-dev \
  cargo test --workspace
```

如果你想执行一次完全干净的重建，可以在 `cargo test --workspace` 前加上 `cargo clean &&`。

## 在容器中打开一个 shell

### Docker

```bash
docker run --rm -it \
  -v "$PWD":/workspace \
  -e CARGO_TARGET_DIR=/tmp/claw-target \
  -w /workspace/rust \
  claw-code-dev
```

### Podman

```bash
podman run --rm -it \
  -v "$PWD":/workspace:Z \
  -e CARGO_TARGET_DIR=/tmp/claw-target \
  -w /workspace/rust \
  claw-code-dev
```

在 shell 中：

```bash
cargo build --workspace
cargo test --workspace
cargo run -p rusty-claude-cli -- --help
cargo run -p rusty-claude-cli -- sandbox
```

`sandbox` 命令是一个很实用的健全性检查：在 Docker 或 Podman 内部，它应该报告 `In container true`，并列出运行时检测到的标记。

## 同时 bind mount 本仓库和另一个仓库

如果你想在保持 `claw-code` 自身以读写方式挂载的同时，让 `claw` 针对第二个检出目录运行：

### Docker

```bash
docker run --rm -it \
  -v "$PWD":/workspace \
  -v "$HOME/src/other-repo":/repo \
  -e CARGO_TARGET_DIR=/tmp/claw-target \
  -w /workspace/rust \
  claw-code-dev
```

### Podman

```bash
podman run --rm -it \
  -v "$PWD":/workspace:Z \
  -v "$HOME/src/other-repo":/repo:Z \
  -e CARGO_TARGET_DIR=/tmp/claw-target \
  -w /workspace/rust \
  claw-code-dev
```

然后，例如：

```bash
cargo run -p rusty-claude-cli -- prompt "summarize /repo"
```

## 说明

- Docker 和 Podman 共用同一个纳入版本控制的 `Containerfile`。
- Podman 示例中的 `:Z` 后缀用于 SELinux 重新标记；在 Fedora/RHEL 这类主机上请保留它。
- 使用 `CARGO_TARGET_DIR=/tmp/claw-target` 可以避免在 bind mount 的检出目录里留下由容器拥有的 `target/` 产物。
- 如果不是在容器中进行本地开发，继续参考 [`../USAGE.md`](../USAGE.md) 和 [`../rust/README.md`](../rust/README.md)。
