# README（LLM Wiki / Obsidian）

> 一个基于 LLM 协作维护的 Obsidian 知识编译系统，用于把原始信息持续编译为可复用、可追溯、可重构的知识资产。

> **跨工具支持**：本项目基于 AGENTS.md 标准，兼容 Codex / Claude Code / Cursor / Gemini CLI 等主流 AI 编程工具。工作流定义在 `skills/` 目录，任何支持 AGENTS.md 的工具均可直接发现并执行。

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

> **工具链**：任意 AGENTS.md 兼容工具（Codex / Claude / opencode / Cursor 等）+ Obsidian（知识库可视化与编辑）。前者负责 `raw → wiki → output` 的编译执行，Obsidian 负责 wikilink 图谱与日常浏览。

---

# 二、系统结构

```text
LLM-WIKI/
├── AGENTS.md                 # 唯一 Agent 规范入口（跨工具）
├── CLAUDE.md                 # Claude Code 入口（指向 AGENTS.md）
├── skills/                   # 跨工具工作流（纯 Markdown）
│   ├── wiki-compile.md        # raw → wiki 提取流程
│   ├── wiki-refactor.md      # 知识复利与重构流程
│   ├── wiki-output.md        # wiki → output 生成流程
│   └── wiki-check.md         # 质量自检流程
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
└── README.md
```

| 层级 | 作用 |
|---|---|
| `raw/` | 原始素材输入层 |
| `wiki/` | 唯一知识真相层 |
| `output/` | 面向人类的表达层 |
| `skills/` | 跨工具工作流提示词 |
| `AGENTS.md` | 系统行为规范（跨工具入口） |

---

# 三、核心工作流

```text
raw → wiki → output
```

> **跨工具执行**：`skills/` 目录下的工作流文件为纯 Markdown，不含任何工具私有语法。在任意支持 AGENTS.md 的工具中，直接要求 LLM 执行对应 skill 即可。

## 工作流速查表

| Skill | 对应流程 | 作用 | 文件 |
|---|---|---|---|
| wiki-compile | ingest | 扫描 `raw/`，将新增/更新的素材编译为 wiki 知识 | `skills/wiki-compile.md` |
| wiki-refactor | refactor | 合并重复、提升抽象层级、构建 systems/comparisons | `skills/wiki-refactor.md` |
| wiki-output | output | 从 wiki 生成 posts / reports / tutorials / slides / newsletters | `skills/wiki-output.md` |
| wiki-check | quality check | 检查 wiki 质量并生成报告 | `skills/wiki-check.md` |

## 1. 收集素材

把未处理材料放入 `raw/inbox/` 或对应分类目录。

示例：

```text
raw/inbox/example.md
raw/articles/example-article.md
raw/papers/example-paper.pdf
```

## 2. 执行 ingest

在任意 LLM 对话中输入：

```text
请执行 skills/wiki-compile.md 中的流程
```

自动对比 `wiki/lifecycle/ingest-registry.md` 登记表，发现 `raw/` 中新增或更新的文件并处理。

典型产物：

- `wiki/summaries/<material>.md`
- `wiki/concepts/<concept>.md`
- `wiki/entities/<entity>.md`
- 更新 `wiki/index.md`
- 追加 `wiki/lifecycle/operation-log.md`

## 3. 定期 refactor

在任意 LLM 对话中输入：

```text
请执行 skills/wiki-refactor.md 中的流程
```

典型操作：

- 合并重复 concept
- 补充 wikilink
- 抽象 system
- 建立 comparison
- 更新 view 和 lifecycle 记录

## 4. 生成 output

在任意 LLM 对话中输入：

```text
请执行 skills/wiki-output.md，类型为 posts      # 文章
请执行 skills/wiki-output.md，类型为 reports    # 报告
请执行 skills/wiki-output.md，类型为 tutorials  # 教程
请执行 skills/wiki-output.md，类型为 slides     # 幻灯片
请执行 skills/wiki-output.md，类型为 newsletters # 简报
```

## 5. 质量检查

在任意 LLM 对话中输入：

```text
请执行 skills/wiki-check.md 中的流程
```

检查全 wiki 质量：归类、可复用性、链接密度、来源追溯、重复检测。

示例：

```text
请基于 wiki/concepts 和 wiki/systems 生成一篇 output/posts 文章
```

---

# 四、最小可用流程

1. 放一份材料到 `raw/inbox/example.md`
2. 要求 LLM 执行 `skills/wiki-compile.md`（自动发现新文件）
3. 检查 `wiki/summaries/`、`wiki/concepts/`、`wiki/entities/`
4. 检查 `wiki/index.md` 和 `wiki/lifecycle/ingest-registry.md`
5. 定期执行 `skills/wiki-refactor.md` 保持知识结构健康
6. 需要输出时，执行 `skills/wiki-output.md`
7. 定期执行 `skills/wiki-check.md` 做质量体检

---

# 五、当前状态

当前仓库已经具备：

- 基础目录骨架
- Agent 行为规范（AGENTS.md + CLAUDE.md）
- 四个跨工具工作流 skill：`wiki-compile` `wiki-refactor` `wiki-output` `wiki-check`（位于 `skills/`）
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
