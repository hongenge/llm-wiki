# README（LLM Wiki / Obsidian）

> 一个基于 LLM 协作维护的 Obsidian 知识编译系统，用于把原始信息持续编译为可复用、可追溯、可重构的知识资产。

---

# 一、项目定位

这个仓库不是传统笔记库，也不是自动化应用程序，而是一个：

> **LLM 参与维护的知识编译系统（Knowledge Compiler System）**

它的核心目标是：

- 将碎片信息结构化为知识
- 将知识持续抽象、合并与重构
- 将知识转化为可复用资产
- 让知识库随着使用不断压缩、连接与升维

> 规范唯一来源：[`AGENTS.md`](AGENTS.md)。README 只说明项目用途、目录和使用方式；所有强制规则以 `AGENTS.md` 为准。

---

# 二、系统结构

```text
LLM-WIKI/
├── AGENTS.md                 # 唯一 Agent 规范入口
├── raw/                      # 原始素材层（只读输入）
│   ├── inbox/                # 临时收集
│   ├── articles/             # 网络文章 / 博客
│   ├── papers/               # 论文 / 白皮书
│   ├── docs/                 # 官方文档
│   ├── transcripts/          # 对话 / 会议 / 播客
│   ├── visual/               # Canvas / Excalidraw / Mermaid 源文件
│   └── assets/               # 图片 / PDF / 数据文件
├── wiki/                     # 核心知识层（LLM 编译产物）
│   ├── index.md              # 全局入口 / MOC 总索引
│   ├── concepts/             # 核心概念
│   ├── entities/             # 实体：工具 / 框架 / 人物 / 系统
│   ├── systems/              # 系统设计 / 方法论组合
│   ├── comparisons/          # 对比分析
│   ├── summaries/            # 材料摘要，中间态
│   ├── views/                # 多维知识视图
│   └── lifecycle/            # 知识演化记录
│       └── operation-log.md
├── output/                   # 成品输出层（人类消费）
│   ├── posts/
│   ├── reports/
│   ├── tutorials/
│   ├── slides/
│   └── newsletters/
├── skills/                   # LLM 能力模块（规则 / 流程）
│   ├── ingest/
│   │   └── ingest-prompt.md  # raw → wiki 提取流程
│   └── reasoning/
│       └── refactor-prompt.md # 知识复利与重构流程
└── README.md
```

| 层级 | 作用 |
|---|---|
| `raw/` | 原始素材输入层 |
| `wiki/` | 唯一知识真相层 |
| `output/` | 面向人类的表达层 |
| `skills/` | LLM 工作流提示词 |
| `AGENTS.md` | 系统行为规范 |

---

# 三、核心工作流

```text
raw → wiki → output
```

> **命令适用范围**：本节的 `/wiki-*` 命令是 **opencode 专属的 slash command**（自定义命令），通过 `.opencode/command/` 下的命令文件提供。如果你使用其他 LLM 客户端或 Obsidian 原生环境，无法直接调用这些命令，但可手动按对应 `skills/` 下的 prompt 流程执行。

## 命令速查表

| 命令 | 对应流程 | 作用 | 来源文件 |
|---|---|---|---|
| `/wiki-compile` | ingest | 扫描 `raw/`，将新增/更新的素材编译为 wiki 知识 | `.opencode/command/wiki-compile.md` |
| `/wiki-refactor [范围/要求]` | refactor | 合并重复、提升抽象层级、构建 systems/comparisons | `.opencode/command/wiki-refactor.md` |
| `/wiki-output <类型>` | output | 从 wiki 生成 posts / reports / tutorials / slides / newsletters | `.opencode/command/wiki-output.md` |
| `/wiki-check [范围]` | quality check | 检查 wiki 质量并生成报告 | `.opencode/command/wiki-check.md` |

## 1. 收集素材

把未处理材料放入 `raw/inbox/` 或对应分类目录。

示例：

```text
raw/inbox/example.md
raw/articles/example-article.md
raw/papers/example-paper.pdf
```

## 2. 执行 ingest

在 opencode 中输入：

```text
/wiki-compile
```

自动对比 `wiki/lifecycle/ingest-registry.md` 登记表，发现 `raw/` 中新增或更新的文件并处理。

典型产物：

- `wiki/summaries/<material>.md`
- `wiki/concepts/<concept>.md`
- `wiki/entities/<entity>.md`
- 更新 `wiki/index.md`
- 追加 `wiki/lifecycle/operation-log.md`

## 3. 定期 refactor

在 opencode 中直接输入：

```text
/wiki-refactor
```

或在任意 LLM 对话中按 [`skills/reasoning/refactor-prompt.md`](skills/reasoning/refactor-prompt.md) 整理 wiki。

典型操作：

- 合并重复 concept
- 补充 wikilink
- 抽象 system
- 建立 comparison
- 更新 view 和 lifecycle 记录

## 4. 生成 output

在 opencode 中直接输入：

```text
/wiki-output posts      # 文章
/wiki-output reports    # 报告
/wiki-output tutorials  # 教程
/wiki-output slides     # 幻灯片
/wiki-output newsletters # 简报
```

## 5. 质量检查

在 opencode 中直接输入：

```text
/wiki-check
```

检查全 wiki 质量：归类、可复用性、链接密度、来源追溯、重复检测。

示例：

```text
请基于 wiki/concepts 和 wiki/systems 生成一篇 output/posts 文章
```

---

# 四、最小可用流程

1. 放一份材料到 `raw/inbox/example.md`
2. 输入 `/wiki-compile`（自动发现新文件）
3. 检查 `wiki/summaries/`、`wiki/concepts/`、`wiki/entities/`
4. 检查 `wiki/index.md` 和 `wiki/lifecycle/ingest-registry.md`
5. 定期输入 `/wiki-refactor` 保持知识结构健康
6. 需要输出时，输入 `/wiki-output posts`
7. 定期输入 `/wiki-check` 做质量体检

---

# 五、当前状态

当前仓库已经具备：

- 基础目录骨架
- Agent 行为规范
- ingest / refactor 流程 prompt
- opencode 专属 slash 命令：`/wiki-compile` `/wiki-refactor` `/wiki-output` `/wiki-check`
- wiki 索引入口
- lifecycle 操作日志

当前仍需要通过真实材料逐步补齐：

- concept / entity / system 样本
- view 组织方式
- output 样本
- 周期性 refactor 习惯

---

# 六、一句话总结

LLM Wiki 的目标不是保存更多笔记，而是让 LLM 持续把原始信息编译为更抽象、更互联、更可复用的知识结构。
