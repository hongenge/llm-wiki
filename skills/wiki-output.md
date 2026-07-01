# wiki-output（wiki → output 生成流程）

> 用途：从 wiki 知识生成面向人类的成品输出
> 数据流：`wiki → output`（禁止 `raw → output`）

---

## 输入

- 输出类型：用户指定 `posts` / `reports` / `tutorials` / `slides` / `newsletters` 之一
- 如未指定类型，先询问用户需要哪种形式

## 执行流程

以 AGENTS.md 第六节"output 生成规则"为规范源。

1. 根据用户指定的类型，确定对应的 wiki 知识来源（concepts / entities / systems / comparisons）。
2. 禁止直接从 `raw/` 引用内容，所有素材必须以 `wiki/` 为唯一知识源。
3. 生成内容时遵循各类型的表达要求：
   - posts：可读性优先
   - reports：结构化分析
   - tutorials：步骤化
   - slides：视觉表达
   - newsletters：信息压缩
4. 产出文件写入 `output/<类型>/` 目录。
5. 从本次输出中反向提取 2 到 5 个可复用 concept，回写到 `wiki/concepts/`。
6. 更新 `wiki/index.md`，登记新增的 wiki 页面或输出页面。
7. 在 `wiki/lifecycle/operation-log.md` 追加 output 记录。

## 产出规则

最小 output 产出：

- `output/<类型>/<文件名>.md`
- 可能的反哺：`wiki/concepts/<新概念>.md`（2~5 个）
- 更新 `wiki/index.md`
- 更新 `wiki/lifecycle/operation-log.md`

每个 output 文件必须标注知识来源（引用了哪些 wiki 页面）。
