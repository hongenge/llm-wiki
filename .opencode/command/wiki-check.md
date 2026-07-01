---
description: 检查 wiki 知识质量（自检清单）
agent: build
---

检查范围或补充要求：

```text
$ARGUMENTS
```

如果 `$ARGUMENTS` 为空，则对全 wiki 做一次综合质量检查。

## 执行流程

以 AGENTS.md 第九节"质量标准"为规范源。

逐项检查每个 wiki 页面是否满足：

1. **归类正确**：是否属于 concept / entity / system / comparison / summary / view 之一？
2. **可复用**：是否可被其他页面引用？还是仅为一次性笔记？
3. **有链接**：是否至少包含一个 `[[wikilink]]`？是否为孤立节点？
4. **可追溯**：是否标注了来源 `raw/` 路径？
5. **无重复**：是否与已有页面内容重叠？

完成后输出一份结构化报告：

```markdown
## 质量检查报告

### 概览
- 检查页面数：N
- 通过：N
- 问题：N

### 问题清单
| 页面 | 问题 | 建议 |
|------|------|------|
| wiki/concepts/xxx | 无来源标注 | 补充 raw/ 引用 |
| wiki/concepts/yyy | 孤立节点 | 添加 [[...]] |
| ... | ... | ... |

### 修复建议
1. ...
2. ...
```

## 产出规则

- 检查报告写入 `output/reports/quality-check-YYYY-MM-DD.md`
- 不直接修改 wiki 页面，仅在报告中给出修复建议
- 更新 `wiki/lifecycle/operation-log.md`
