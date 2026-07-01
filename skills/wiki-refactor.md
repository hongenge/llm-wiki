# wiki-refactor（知识复利与重构流程）

> 用途：合并重复知识、提升抽象层级、构建 systems、清理冗余
> 核心原则：知识复利——每次操作让 wiki 更压缩、更连接、更升维

---

## 输入

- 现有 `wiki/concepts/`、`wiki/entities/`、`wiki/summaries/` 全集
- 可选：用户指定的重构范围或补充要求（如无则默认对当前 `wiki/` 做一次全局结构体检和小步重构）

## 复利操作类型（每次至少满足一个）

| 操作 | 说明 |
|---|---|
| 提升抽象层级 | 多个具体 concept → 抽象 concept |
| 增加交叉引用 | 补充 `[[wikilink]]`，消除孤立节点 |
| 合并重复知识 | merge：A + B → C，A/B 标注 superseded_by |
| 提升可复用性 | 抽取通用模式，去上下文化 |

## 执行流程

1. 扫描已有 wiki 页面，标记重叠、孤岛和弱抽象。
2. 禁止修改 `raw/`。
3. 有充分理由时，合并重复或重叠页面。
4. 被合并页用 `> [SUPERSEDED] merged into [[目标页面]] on YYYY-MM-DD` 标注，不物理删除。
5. 存在矛盾时，保留冲突并标注来源，不强行统一。
6. 补全缺失的 wikilinks，增强交叉引用。
7. 多页面共享可复用模式时，提取为更高层的 concept 或 system。
8. 两个概念或实体存在替代关系和取舍时，建立 comparison。
9. 需要多维索引时，更新 `wiki/views/`。
10. 更新 `wiki/index.md`，在 `wiki/lifecycle/operation-log.md` 追加生命周期记录。

## 产出规则

重构可能产出：

- 重写的 `wiki/concepts/` 或 `wiki/entities/` 页面
- 新建的 `wiki/systems/` 页面
- 新建的 `wiki/comparisons/` 页面
- 更新的 `wiki/views/` 页面
- 标注了合并标记的被替代页面
- 更新 `wiki/index.md`
- 更新 `wiki/lifecycle/operation-log.md`

每次重构必须至少提升以下一项：

- 抽象层级
- 交叉链接密度
- 去重程度
- 可复用性

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
