# Claw Code Philosophy
# Claw Code 哲学

## Stop Staring at the Files
## 别再盯着文件本身看了

If you only look at the generated files in this repository, you are looking at the wrong layer.
如果你只看这个仓库里生成出来的文件，那你看的层级就错了。

The Python rewrite was a byproduct. The Rust rewrite was also a byproduct. The real thing worth studying is the **system that produced them**: a clawhip-based coordination loop where humans give direction and autonomous claws execute the work.
Python 重写是副产物。Rust 重写也是副产物。真正值得研究的是**产生它们的系统**：一个基于 clawhip 的协作循环，由人类提供方向，由自主 claws 执行工作。

Claw Code is not just a codebase. It is a public demonstration of what happens when:
Claw Code 不只是一个代码库。它是一个公开展示，说明当以下条件成立时会发生什么：

- a human provides clear direction,
- 人类提供清晰的方向，
- multiple coding agents coordinate in parallel,
- 多个 coding agents 并行协作，
- notification routing is pushed out of the agent context window,
- 通知路由被移出 agent 的上下文窗口，
- planning, execution, review, and retry loops are automated,
- 规划、执行、审查与重试循环被自动化，
- and the human does **not** sit in a terminal micromanaging every step.
- 而且人类**不需要**坐在终端前对每一步进行微观管理。

## The Human Interface Is Discord
## 人类界面是 Discord

The important interface here is not tmux, Vim, SSH, or a terminal multiplexer.
这里真正重要的界面不是 tmux、Vim、SSH，也不是终端复用器。

The real human interface is a Discord channel.
真正的人类界面是一个 Discord 频道。

A person can type a sentence from a phone, walk away, sleep, or do something else. The claws read the directive, break it into tasks, assign roles, write code, run tests, argue over failures, recover, and push when the work passes.
一个人可以在手机上输入一句话，然后离开、睡觉，或者去做别的事。claws 会读取指令，将其拆成任务，分配角色，编写代码，运行测试，对失败进行争论，完成恢复，并在工作通过后进行 push。

That is the philosophy: **humans set direction; claws perform the labor.**
这就是其哲学：**人类设定方向；claws 负责执行。**

## The Three-Part System
## 三部分系统

### 1. OmX (`oh-my-codex`)
### 1. OmX（`oh-my-codex`）
[oh-my-codex](https://github.com/Yeachan-Heo/oh-my-codex) provides the workflow layer.
[oh-my-codex](https://github.com/Yeachan-Heo/oh-my-codex) 提供工作流层。

It turns short directives into structured execution:
它把简短的指令转化为结构化执行：
- planning keywords
- planning 关键字
- execution modes
- execution 模式
- persistent verification loops
- 持续验证循环
- parallel multi-agent workflows
- 并行 multi-agent 工作流

This is the layer that converts a sentence into a repeatable work protocol.
这是把一句话转化为可重复工作协议的那一层。

### 2. clawhip
### 2. clawhip
[clawhip](https://github.com/Yeachan-Heo/clawhip) is the event and notification router.
[clawhip](https://github.com/Yeachan-Heo/clawhip) 是事件与通知路由器。

It watches:
它负责监控：
- git commits
- git commits
- tmux sessions
- tmux sessions
- GitHub issues and PRs
- GitHub issues 和 PRs
- agent lifecycle events
- agent 生命周期事件
- channel delivery
- 频道投递

Its job is to keep monitoring and delivery **outside** the coding agent's context window so the agents can stay focused on implementation instead of status formatting and notification routing.
它的职责是把监控与投递保持在 coding agent 上下文窗口的**外部**，这样 agents 就能专注于实现，而不是处理状态格式化和通知路由。

### 3. OmO (`oh-my-openagent`)
### 3. OmO（`oh-my-openagent`）
[oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent) handles multi-agent coordination.
[oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent) 负责 multi-agent 协调。

This is where planning, handoffs, disagreement resolution, and verification loops happen across agents.
这里是跨 agents 进行规划、交接、分歧解决与验证循环的地方。

When Architect, Executor, and Reviewer disagree, OmO provides the structure for that loop to converge instead of collapse.
当 Architect、Executor 和 Reviewer 出现分歧时，OmO 提供让这个循环收敛而不是崩溃的结构。

## The Real Bottleneck Changed
## 真正的瓶颈已经改变

The bottleneck is no longer typing speed.
瓶颈已经不再是打字速度。

When agent systems can rebuild a codebase in hours, the scarce resource becomes:
当 agent 系统能在数小时内重建一个代码库时，稀缺资源变成了：
- architectural clarity
- 架构清晰度
- task decomposition
- 任务拆解能力
- judgment
- 判断力
- taste
- 品味
- conviction about what is worth building
- 对什么值得构建的确信
- knowing which parts can be parallelized and which parts must stay constrained
- 知道哪些部分可以并行化，哪些部分必须受限

A fast agent team does not remove the need for thinking. It makes clear thinking even more valuable.
一个快速的 agent 团队并不会消除思考的必要性。它只会让清晰思考变得更加有价值。

## What Claw Code Demonstrates
## Claw Code 展示了什么

Claw Code demonstrates that a repository can be:
Claw Code 展示了一个仓库可以是这样的：

- **autonomously built in public**
- **在公开环境中自主构建**
- coordinated by claws/lobsters rather than human pair-programming alone
- 由 claws/lobsters 协调，而不只是依赖人类结对编程
- operated through a chat interface
- 通过聊天界面进行操作
- continuously improved by structured planning/execution/review loops
- 通过结构化的规划/执行/审查循环持续改进
- maintained as a showcase of the coordination layer, not just the output files
- 被维护为协作层的展示，而不只是输出文件的集合

The code is evidence.
代码是证据。
The coordination system is the product lesson.
协作系统才是产品层面的启示。

## What Still Matters
## 仍然重要的是什么

As coding intelligence gets cheaper and more available, the durable differentiators are not raw coding output.
随着 coding intelligence 变得更便宜、更普及，持久的差异化因素不再是原始代码产出。

What still matters:
仍然重要的是：
- product taste
- 产品品味
- direction
- 方向
- system design
- 系统设计
- human trust
- 人类信任
- operational stability
- 运行稳定性
- judgment about what to build next
- 对接下来该构建什么的判断

In that world, the job of the human is not to out-type the machine.
在那样的世界里，人类的工作不是比机器打字更快。
The job of the human is to decide what deserves to exist.
人类的工作是决定什么值得存在。

## Short Version
## 简短版

**Claw Code is a demo of autonomous software development.**
**Claw Code 是一个自主软件开发的演示。**

Humans provide direction.
人类提供方向。
Claws coordinate, build, test, recover, and push.
Claws 负责协调、构建、测试、恢复并 push。
The repository is the artifact.
仓库是产物。
The philosophy is the system behind it.
哲学则是其背后的系统。

## Related explanation
## 相关说明

For the longer public explanation behind this philosophy, see:
关于这一哲学更长的公开说明，请参见：

- https://x.com/realsigridjin/status/2039472968624185713
- https://x.com/realsigridjin/status/2039472968624185713
