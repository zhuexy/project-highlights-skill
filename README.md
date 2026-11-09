# Project Highlights

`project-highlights` 是一个用于后端项目分析和简历素材整理的 Codex skill。它帮助你从项目代码中发现值得深入理解的技术设计，核实实现证据，再把选定亮点整理成 STAR 讲述稿和简历素材。

## 适用场景

- 刚加入一个后端项目，希望快速建立项目地图并找到核心业务链路。
- 想从项目设计中发掘值得学习、准备面试或写入项目介绍的技术亮点。
- 已选定某个亮点，希望追踪完整调用链、分析方案取舍和异常边界。
- 想把已经确认的个人贡献整理成 STAR 和简历表述。

Skill 不预设 Java 或特定框架，会根据项目内容识别技术栈。默认只读分析业务源码；项目分析材料保存在仓库外的用户级 Codex 数据目录，避免意外提交到业务仓库。

默认位置是 `$CODEX_HOME/project-highlights/<project-id>/`；如果没有设置 `CODEX_HOME`，则使用 `$HOME/.codex/project-highlights/<project-id>/`（Windows 位于用户 profile 下的 `.codex`）。项目标识由仓库名和规范化 Git remote URL 的 SHA-256 前缀组成；没有 remote 时改用规范化仓库根路径。这样同名仓库也会分开保存。索引会记录项目路径和标识，供以后续接。

## 使用方法

先安装 skill，再在 Codex 中打开要分析的后端项目并调用 `$project-highlights`。

### 安装

可以让 Codex 的 `$skill-installer` 从 GitHub 安装：

```text
$skill-installer 从 https://github.com/zhuexy/project-highlights-skill 安装 SKILL.md 所在的 skill。
```

或者将仓库克隆到某个项目的 `.agents/skills/` 目录下：

```bash
git clone https://github.com/zhuexy/project-highlights-skill.git .agents/skills/project-highlights
```

第二种方式将 skill 限定在该项目中；通过 `$skill-installer` 安装则放入用户级 Codex skills 目录。安装后按当前 Codex 环境提示启用 skill，再打开目标业务项目使用。

### 调用

可以直接描述目标，也可以使用下面的提示词：

```text
$project-highlights 分析当前后端项目，先给出值得深入理解和写进项目经历的技术亮点候选，按技术深度与面试可讲性排序。列出代码证据、推荐理由和待确认事项，等我选择后再深入分析。
```

如果已经有心仪的方向，可以指定它：

```text
$project-highlights 深入分析订单支付回调中的幂等处理，追踪完整调用链、并发和失败边界，并在访谈后生成 STAR 素材。
```

继续之前的分析：

```text
$project-highlights 读取已有的 project-highlights 材料，从上次进度继续。
```

如果希望结果针对某个岗位排序，可附上 JD；这是可选信息。

## 工作流程

1. **发现候选亮点**：读取项目说明、模块、入口和核心数据，建立精简项目地图，再沿相关业务链路寻找候选。通常给出 5～10 项；小项目会按实际情况减少，不会为了凑数而添加项目。
2. **你选择后再深挖**：候选项包含业务问题、设计机制、代码证据、技术难点、推荐理由和证据缺口。选定后，skill 分析调用链、业务约束、异常与并发边界、方案取舍和验证情况。
3. **访谈并整理 STAR**：先查项目材料，再逐个询问代码无法确认的个人职责、行动和结果。已确认个人贡献时，按真实职责生成个人 STAR；缺少指标时不会编造数字。

## Owner 视角 STAR

如果你明确表示没有参与亮点实现，或无法解释其基本设计，skill 会停止追问个人贡献，并直接生成完整的 **S/T/A/R 项目 Owner 视角复盘**：从负责项目的视角说明业务背景、目标、设计与决策、已证实的结果。

这份 STAR 是一种项目复盘和面试准备材料，不代表你真实担任过 Owner，也不能直接作为你的个人履历提交。只有你确认过的实际贡献才会写成个人简历经历。Owner 视角下的设计分析和结果仍会标注代码证据与验证边界。

## 分析产物

按需创建以下文件，不会预先生成空文件：

```text
<user-codex-home>/project-highlights/<project-id>/
  index.md
  H01-payment-callback/
    analysis.md
    interview.md
    resume.md
```

- `index.md`：项目地图、亮点候选、当前阶段和待办。
- `analysis.md`：选定亮点的业务背景、调用链、设计机制、取舍、边界和证据。
- `interview.md`：已确认的个人经历、回答来源和待确认事项。
- `resume.md`：个人 STAR 或 Owner 视角 STAR、简历候选表述、证据映射及补充材料。

亮点 ID 在创建后保持稳定。后续调用会读取这些材料以接续分析。

## 分析原则

- 不仅检查你亲自实现的模块，也分析项目整体设计；明确区分项目设计、个人贡献和验证结果。
- 重要技术结论尽可能对应到文件、类或函数。依赖、类名和注解本身不能证明机制有效。
- 区分已证实、推测和待确认内容；区分代码存在、测试通过、验收交付和生产观察。
- 不猜测个人职责、线上规模或性能收益，也不把 Owner 视角复盘伪装成个人经历。
- 不因亮点分析自动修改业务代码、运行负载测试或连接外部服务。

## Repository Layout

- [`SKILL.md`](SKILL.md): skill instructions and workflow.
- [`references/analysis.md`](references/analysis.md): candidate discovery and technical verification guidance.
- [`references/artifacts.md`](references/artifacts.md): project analysis and STAR artifact formats.
- [`agents/openai.yaml`](agents/openai.yaml): Codex UI metadata.
