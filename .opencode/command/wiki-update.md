---
description: 重构 LLM Wiki，合并重复知识并提升抽象层级
agent: build
---

你的任务不是生成新知识，而是：在已有 wiki 知识库基础上进行增量修正与优化。

重构范围或补充要求：

```text
$ARGUMENTS
```

如果 `$ARGUMENTS` 为空，则默认对当前 `wiki/` 做一次全局结构体检和小步重构。

## 执行流程

以 `skills/reasoning/refactor-prompt.md` 为权威流程源。

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
