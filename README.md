# Project Highlights

`project-highlights` 是一个用于项目分析和简历素材整理的 Codex skill。它从代码和项目材料中并列挖掘技术亮点、业务亮点和未来演进机会，核实实现证据，并为选定候选整理项目 Owner 视角 STAR。

## 适用场景

- 刚加入一个后端项目，希望快速建立项目地图并找到核心业务链路。
- 想从项目设计和业务规则中发掘技术或业务亮点，用于项目介绍、复盘或简历素材。
- 已选定某个亮点，希望追踪完整调用链、分析方案取舍和异常边界。
- 想以项目 Owner 视角整理各亮点的 STAR，并继续深挖重点候选。

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
$project-highlights 分析当前项目，分别挖掘技术亮点、业务亮点和未来演进机会并排序。先只生成 `index.md`，每项列出代码证据、推荐理由、当前问题/机会、触发条件和待验证实验；不要访谈，等我选择亮点后再深挖。
```

如果已经有心仪的方向，可以指定它：

```text
$project-highlights 深入分析订单支付回调中的幂等处理，追踪完整调用链、并发和失败边界，更新项目 Owner 视角 STAR 与证据映射。
```

继续之前的分析：

```text
$project-highlights 读取已有的 project-highlights 材料，从上次进度继续。
```

如果希望结果针对某个岗位排序，可附上 JD；这是可选信息。

## 工作流程

1. **发现候选亮点**：读取项目说明、模块、入口和核心数据，建立精简项目地图，再沿相关业务链路寻找候选。通常给出 5～10 项；小项目会按实际情况减少，不会为了凑数而添加项目。
2. **先读索引再选择**：阶段一只生成项目级 `index.md`，其中列出技术/业务/未来演进候选、代码证据、推荐理由、证据缺口、触发条件和实验建议，不生成各亮点目录或完整 STAR。你阅读索引后选择想深挖的亮点，skill 才分析调用链、业务约束、异常与并发边界、方案取舍和验证情况。
3. **整理 Owner 视角 STAR**：选定亮点深挖后生成 STAR。`resume.md` 固定提供“版本 A：架构治理型”和“版本 B：问题闭环型”，两个版本都围绕具体问题写清 S/T/A/R、行动和结果。每个待验证主张都会附实验/验证建议，说明对照、操作、量化指标和判定方式；方案尚未执行时会明确标出。

## Owner 视角 STAR

Skill 默认直接生成完整的 **S/T/A/R 项目 Owner 视角复盘**：从项目负责者视角说明业务背景、目标、设计与决策、已证实的结果。

这份 STAR 是一种项目复盘和面试准备材料，不自动代表你真实担任过 Owner 或亲自完成了相关行动。可以将设计理由重构为合理推断，但会明确标注；代码不能证明的实验结论、量化收益和业务效果不会写成已发生事实。Owner 视角下的设计分析和结果仍会标注证据与验证边界。

## 分析产物

按需创建以下文件，不会预先生成空文件：

```text
<user-codex-home>/project-highlights/<project-id>/
  index.md
  H01-payment-callback/
    analysis.md
    resume.md
```

- `index.md`：项目地图、亮点候选、当前阶段和待办。
- `analysis.md`：选定亮点的业务背景、调用链、设计机制、取舍、边界和证据。
- `resume.md`：Owner 视角 STAR、简历候选表述、证据映射及待验证材料。

亮点 ID 在创建后保持稳定。后续调用会读取这些材料以接续分析。

## 分析原则

- 不仅检查你亲自实现的模块，也分析项目整体设计；明确区分项目设计、个人贡献和验证结果。
- 重要技术结论尽可能对应到文件、类或函数。依赖、类名和注解本身不能证明机制有效。
- 区分已证实、推测和待确认内容；区分代码存在、测试通过、验收交付和生产观察。
- 待验证结论附可操作的补证指导：技术性能主张会按对象推荐工具和配置（如 API QPS 用 JMeter/k6/wrk、JVM 微基准用 JMH），业务效果建议 A/B 或适当的准实验，并定义指标、样本/周期依据及局限。
- 未来演进机会必须有当前代码证据，例如低并发假设、分支膨胀、单点依赖或缺少容量保护；材料中说明触发条件、目标方案、迁移/回滚路径和验证方法，当前状态标为未实现。
- 不虚构个人职责、线上规模、实验结论或性能收益；Owner 视角复盘明确标记为项目角色视角。
- 不因亮点分析自动修改业务代码、运行负载测试或连接外部服务。

## Repository Layout

- [`SKILL.md`](SKILL.md): skill instructions and workflow.
- [`references/analysis.md`](references/analysis.md): candidate discovery and technical verification guidance.
- [`references/artifacts.md`](references/artifacts.md): project analysis and STAR artifact formats.
- [`agents/openai.yaml`](agents/openai.yaml): Codex UI metadata.
