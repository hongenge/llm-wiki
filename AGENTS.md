# AGENTS.md（LLM Wiki 规范 / Obsidian 版本）

> 本文件是 **LLM Wiki 系统唯一行为规范源（Single Source of Truth）**
> 所有 LLM 在执行 **读取 / 编译 / 写入 / 重构 / 输出** 前必须遵循本规范
> 适用于 Obsidian 知识库长期演进系统

---

# 一、系统架构约定

```text
LLM-WIKI/
├── AGENTS.md
├── .opencode/   # opencode 原生集成（command）
├── raw/        # 原始素材（只读）
├── wiki/       # 知识编译层（LLM读写）
├── output/     # 成品输出（LLM生成 + 人类审阅）
├── skills/     # LLM能力模块（规则/流程）
```

---

## 层级职责表

| 层级     | 路径        | 所有者      | 权限    | 说明     |
| ------ | --------- | -------- | ----- | ------ |
| Schema | AGENTS.md | 人类 + LLM | LLM只读 | 行为规则   |
| Raw    | raw/      | 人类       | LLM只读 | 原始信息源  |
| Wiki   | wiki/     | LLM      | 读写    | 知识核心层  |
| Output | output/   | LLM + 人类 | 读写    | 对外表达结果 |
| Skills | skills/   | 人类       | LLM只读 | 工作流规则  |

---

# 二、核心原则（必须严格遵守）

## 1. 原始素材不可变

* ❌ 禁止修改 `raw/`
* ❌ 禁止删除 `raw/`
* ✔ raw 仅作为输入源

---

## 2. Wiki 是唯一知识真相层

* 所有知识必须最终落在 `wiki/`
* wiki 内容必须可追溯到 raw
* wiki 可被重写（refactor 是允许且推荐的）

---

## 3. 知识复利原则（核心）

每次写入必须至少满足一个：

* 提升抽象层级
* 增加交叉引用
* 合并重复知识
* 提升可复用性

---

## 4. 强制双链机制

所有 wiki 内容必须包含：

* `[[Concept]]` 链接
* 或 `[[Entity]]` 链接

禁止孤立节点

---

## 5. 矛盾透明原则

当不同来源冲突时：

必须：

* 保留冲突
* 标注来源
* 不强行统一结论

示例：

```md
观点A（来源：paper X）
观点B（来源：article Y）
→ 当前未统一
```

---

## 6. 生命周期记录必须完整

所有重要操作必须记录：

* merge
* rewrite
* delete（逻辑删除）
* refactor
* ingest

记录位置：

```text
wiki/lifecycle/operation-log.md
```

新增、合并、重写、逻辑删除或输出反哺时，还必须同步更新：

```text
wiki/index.md
```

`wiki/index.md` 是全局入口和登记表，不承载重复知识，只登记页面、摘要、关系和状态。

---

## 7. 渐进式演化原则

* schema 可进化
* concept 可重写
* system 可重构
* 但必须保留历史痕迹

---

# 三、数据流规则（非常重要）

## 标准流向

```text
raw → wiki → output
```

---

## 禁止流向

* ❌ raw → output（禁止）
* ❌ output → wiki（除 refactor 外禁止）
* ❌ wiki → raw（禁止）

---

# 四、Wiki 写入规则（LLM执行规范）

## 命名约定

* 类型名使用单数：concept / entity / system / comparison / summary / view
* 目录名使用复数：concepts / entities / systems / comparisons / summaries / views
* 页面标题应使用稳定概念名或实体名，避免一次性描述句

---

## 1. concept（最重要单元）

每个 concept 必须满足：

* 单一主题
* 可解释
* 可复用
* 可链接

结构：

```md
# Concept Name

## Definition
...

## Key Points
...

## Related
- [[...]]
```

---

## 2. entity（实体）

用于：

* 工具
* 框架
* 系统
* 人物
* 技术组件

要求：

* 必须是“可识别对象”
* 必须可链接

---

## 3. systems（系统层）

用于组合多个 concept：

* 架构设计
* 工程系统
* 方法论体系

要求：

* 明确输入 concept
* 明确组合逻辑

---

## 4. comparisons（对比）

必须包含：

* A vs B
* 使用场景
* trade-off

---

## 5. summaries（摘要）

注意：

> summaries 只是中间态，不是知识终态

必须：

* 可追溯
* 可升级为 concept

---

## 6. views（知识视图）

用于：

* MOC 替代
* 多维度组织知识

特点：

* 不重复知识
* 只做索引

---

## 7. 逻辑删除与合并

wiki 内容原则上不物理删除。废弃页面必须保留历史痕迹，并在页面顶部标注：

```md
> [SUPERSEDED] merged into [[Target Page]] on YYYY-MM-DD
```

同时必须：

* 将有效内容合并到目标页
* 在 `wiki/index.md` 更新状态或移除活跃登记
* 在 `wiki/lifecycle/operation-log.md` 记录 delete / merge 操作

---

# 五、raw 处理规则（ingestion）

## 输入必须经过 LLM 处理：

### 标准流程：

```text
raw content → extract concepts → write wiki
```

---

## LLM必须执行：

* 提取 3~7 个 concepts
* 去冗余
* 建立链接建议
* 标注来源

---

## 最小 ingest 闭环示例

输入：

```text
raw/inbox/example.md
```

输出至少包括：

```text
wiki/summaries/example.md
wiki/concepts/<concept>.md
wiki/entities/<entity>.md
wiki/index.md
wiki/lifecycle/operation-log.md
wiki/lifecycle/ingest-registry.md
```

要求：

* summary 必须注明来源 `raw/inbox/example.md`
* concept / entity 必须包含 `[[...]]` 双链
* `wiki/index.md` 必须登记新增页面
* `operation-log.md` 必须记录 ingest 操作
* `ingest-registry.md` 必须追加该素材的登记条目（路径、时间、产出页面）

---

# 六、output 生成规则

output 是“表达层”，必须：

## 类型要求

* posts：可读性优先
* reports：结构化分析
* tutorials：步骤化
* slides：视觉表达
* newsletters：信息压缩

---

## 必须行为：

* 从 wiki 引用知识
* 不直接使用 raw
* 每个 output 必须反哺 wiki（至少 2–5 concepts）

---

# 七、skills 模块规则

skills 是“能力定义”，不是知识库

## ingest/

负责：

* raw → wiki
* 信息拆解
* concept 提取

## reasoning/

负责：

* concept 合并
* system 构建
* 知识抽象

> 知识图谱可视化由 Obsidian 内置 Graph View 提供，不单独设 skill。
>
> ## opencode 集成
>
> 项目提供 opencode slash command，位于 `.opencode/`：
>
> * `.opencode/command/wiki-compile.md` — `/wiki-compile` 命令，用户直接触发
> * `.opencode/command/wiki-refactor.md` — `/wiki-refactor` 命令，用户直接触发
> * `.opencode/command/wiki-output.md` — `/wiki-output <类型>` 命令，用户直接触发
> * `.opencode/command/wiki-check.md` — `/wiki-check` 命令，用户直接触发
>
> opencode command 的权威流程源仍为 `skills/` 下的 prompt 文件。两者不一致时以 `skills/` 为准。

---

# 八、日志系统（必须执行）

所有关键操作必须记录：

路径：

```text
wiki/lifecycle/operation-log.md
```

必须包含：

* 时间
* 输入来源
* 修改内容
* 输出结果
* reason（可选）

---

# 九、质量标准（LLM自检）

写入 wiki 前必须检查：

* [ ] 是否属于 concept / entity / system
* [ ] 是否可复用
* [ ] 是否有 link
* [ ] 是否来源可追溯 raw
* [ ] 是否避免重复

---

# 十、最终原则（一句话）

> LLM Wiki 的本质不是“记录知识”，而是持续重构知识结构，使其不断压缩、连接与升维。

---

如果你下一步要升级，我可以帮你做一个更关键的东西：

> 👉 二件套（已落地，直接可用）：
> - **ingest prompt**：[`skills/ingest/ingest-prompt.md`](skills/ingest/ingest-prompt.md) — raw → wiki 提取流程
> - **refactor prompt**：[`skills/reasoning/refactor-prompt.md`](skills/reasoning/refactor-prompt.md) — 知识复利与重构流程
>
> 知识图谱可视化由 Obsidian 内置 Graph View 提供，无需独立 prompt。
