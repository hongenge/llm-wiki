# Ingest Prompt（raw → wiki 提取流程）

> 用途：把 `raw/` 中的原始素材拆解为 concepts，写入 `wiki/`
> 触发：用户提供新素材放入 `raw/`，或显式要求"消化/整理/ingest 某文件"
> 数据流：`raw → wiki`（禁止 `raw → output`）

---

## 输入

- 素材路径：`raw/<subdir>/<file>`（subdir ∈ inbox/articles/papers/docs/transcripts/visual/assets）
- 可选：用户指明的关注焦点

## 输出

- `wiki/summaries/<material>.md`（材料摘要，中间态）
- `wiki/entities/<entity>.md`（实体页，可识别对象）
- `wiki/concepts/<concept>.md`（概念页，最重要单元）
- `wiki/lifecycle/operation-log.md` 追加一行操作记录

## 执行步骤

1. **读取素材**：完整读取 `raw/` 指定文件，禁止修改原文
2. **提取知识**：识别 3~7 个 concepts、相关 entities、核心论点、数据、结论
3. **去冗余**：与现有 wiki 比对，重复内容不新建页，改为补充链接与证据
4. **建立双链**：每个新页必须含 `[[Concept]]` 或 `[[Entity]]`，禁止孤立节点
5. **标注来源**：每页 frontmatter 或正文注明 `source: raw/<path>`
6. **处理矛盾**：若与现有 wiki 冲突，按"矛盾透明原则"保留双方并标注来源，不强行统一
7. **写 summaries**：先写材料级摘要页（中间态，可后续升级为 concept）
8. **写 entities/concepts**：满足"被 2+ 材料提及"的实体/概念才独立成页
9. **更新 index.md**：新页加入对应分区表格
10. **记录日志**：在 `wiki/lifecycle/operation-log.md` 追加一行

## 写入前自检（AGENTS.md 第九节）

- [ ] 是否属于 concept / entity / system
- [ ] 是否可复用
- [ ] 是否有 link（强制双链）
- [ ] 是否来源可追溯 raw
- [ ] 是否避免重复

## Concept 页结构模板

```md
# <Concept Name>

## Definition
<一句话定义>

## Key Points
- ...
- ...

## Related
- [[...]]
- [[...]]

## Source
- raw/<path>
```

## Entity 页结构模板

```md
# <Entity Name>

## Type
<工具/框架/系统/人物/技术组件>

## Overview
<可识别对象说明>

## Related
- [[...]]

## Source
- raw/<path>
```
