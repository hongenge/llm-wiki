# Refactor Prompt（知识复利与重构流程）

> 用途：合并重复知识、提升抽象层级、构建 systems、清理冗余
> 触发：定期整理、concept merge、重复清理、用户要求"重构/refactor/合并"
> 核心原则：知识复利——每次操作让 wiki 更压缩、更连接、更升维

---

## 输入

- 现有 `wiki/concepts/`、`wiki/entities/`、`wiki/summaries/` 全集
- 可选：用户指定的合并目标或主题

## 输出

- 合并/重写后的 concept/entity 页
- 新建 `wiki/systems/<system>.md`（组合型知识）
- 新建 `wiki/comparisons/<a-vs-b>.md`（对比分析）
- `wiki/lifecycle/operation-log.md` 记录 merge/rewrite/delete/refactor/ingest

## 复利操作类型（每次至少满足一个）

| 操作 | 说明 |
|---|---|
| 提升抽象层级 | 多个具体 concept → 抽象 concept |
| 增加交叉引用 | 补充 `[[wikilink]]`，消除孤立节点 |
| 合并重复知识 | merge：A + B → C，A/B 标注 superseded_by |
| 提升可复用性 | 抽取通用模式，去上下文化 |

## 执行步骤

1. **扫描重复**：识别主题重叠的 concept/entity 对
2. **合并（merge）**：
   - 保留更抽象/更完整的一页为主体
   - 被合并页改为逻辑删除：顶部加 `> [SUPERSEDED] merged into [[<target>]] on <date>`
   - 主体页吸收证据与链接
3. **抽象升维**：若多个 concept 共享模式，抽取为更高层 concept 或 system
4. **构建 systems**：组合多个 concept 形成架构/方法论体系
   - system 页须明确：输入 concept + 组合逻辑
5. **构建 comparisons**：对可替代的 entity/concept 做 A vs B 对比
   - 必须含：使用场景 + trade-off
6. **更新 views**：多维索引重组（不重复知识，只做索引）
7. **更新 index.md**：反映合并/新增
8. **记录 lifecycle**：merge/rewrite/delete/refactor/ingest 均需在 `wiki/lifecycle/operation-log.md` 记录

## Systems 页结构模板

```md
# <System Name>

## Purpose
<系统目标>

## Input Concepts
- [[...]]
- [[...]]

## Composition Logic
<如何组合>

## Related
- [[...]]
```

## Comparisons 页结构模板

```md
# <A> vs <B>

## A
...

## B
...

## Use Cases
- A 适合：...
- B 适合：...

## Trade-off
- A 优势 / 劣势
- B 优势 / 劣势

## Related
- [[...]]
```

## 生命周期记录格式

所有 merge/rewrite/delete/refactor/ingest 必须在 `wiki/lifecycle/operation-log.md` 追加：

| 时间 | 操作类型 | 输入来源 | 修改内容 | 输出结果 | reason |
|---|---|---|---|---|---|
| YYYY-MM-DD | merge | [[A]],[[B]] | 合并重复概念 | [[C]] | 主题重叠 |
