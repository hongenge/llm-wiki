# wiki-compile（raw → wiki 提取流程）

> 用途：把 `raw/` 中的原始素材拆解为 concepts，写入 `wiki/`
> 数据流：`raw → wiki`（禁止 `raw → output`）

---

## 执行逻辑

1. 递归扫描 `raw/` 下所有文件（排除 `.gitkeep` 等占位文件）。
2. 读取 `wiki/lifecycle/ingest-registry.md`，获取已收录路径列表及上次 ingest 时间。
3. 对比扫描结果与登记表，筛选出：
   - **新文件**：在 `raw/` 中存在但登记表中无记录的
   - **更新文件**：登记表中有记录，但文件的最后修改时间晚于登记表中的"上次 ingest"时间
4. 将筛选结果以列表形式输出，标注每个文件的状态（新/更新）。
5. 对筛选出的文件，**逐个**按下方"执行流程"执行 ingest。
6. 每个文件处理完毕后，立即更新登记表。
7. 如果没有任何新文件或更新文件，告知用户"所有 raw 素材均已处理"。

## 执行流程

1. 完整阅读素材内容。
2. 禁止修改 `raw/` 下任何内容。
3. 提取 3 到 7 个可复用概念。
4. 存在可识别实体时，一并提取。
5. 与已有 `wiki/` 页面比对，避免重复。
6. 存在矛盾时，保留冲突并标注来源，不强行统一。
7. 根据需要创建或更新 `wiki/summaries/`、`wiki/concepts/`、`wiki/entities/`。
8. 添加 Obsidian wikilinks，确保无孤立页面。
9. 更新 `wiki/index.md`，登记新增或变更的页面。
10. 在 `wiki/lifecycle/operation-log.md` 追加 ingest 记录。
11. 在 `wiki/lifecycle/ingest-registry.md` 追加或更新该素材的登记条目（路径、ingest 时间、产出页面列表）。

## 写入前自检（AGENTS.md 第九节）

- [ ] 是否属于 concept / entity / system
- [ ] 是否可复用
- [ ] 是否有 link（强制双链）
- [ ] 是否来源可追溯 raw
- [ ] 是否避免重复

## 产出规则

最小 ingest 产出：

- `wiki/summaries/<素材名>.md`
- `wiki/concepts/<概念>.md`（提取到可复用概念时）
- `wiki/entities/<实体>.md`（提取到可识别实体时）
- 更新 `wiki/index.md`
- 更新 `wiki/lifecycle/operation-log.md`
- 更新 `wiki/lifecycle/ingest-registry.md`

每篇生成的 wiki 页面必须包含：

- 对 `raw/` 路径的来源标注
- 至少一个 `[[wikilink]]`
- 可复用的概念/实体/系统框架，而非一次性笔记堆砌

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
