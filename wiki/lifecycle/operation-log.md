# Operation Log

> 所有重要操作（merge / rewrite / delete / refactor / ingest）必须在此追加一行。
> 字段：时间 | 操作类型 | 输入来源 | 修改内容 | 输出结果 | reason

---

| 时间 | 操作 | 输入来源 | 修改内容 | 输出结果 | reason |
|------|------|----------|----------|----------|--------|
| 2026-06-30 | init | — | 初始化 LLM Wiki 系统结构，创建目录骨架与二件套 prompt | wiki/index.md, wiki/lifecycle/operation-log.md, skills/ingest\|reasoning | 完成中文 Obsidian 重设计补全 |
| 2026-07-01 | rewrite | README.md, AGENTS.md | 收敛 README 为入口说明，补充 AGENTS 的 index 同步、命名约定、逻辑删除和最小 ingest 闭环 | README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 降低规范漂移风险，增强系统可执行性 |
| 2026-07-01 | refactor | skills/ingest/ingest-prompt.md, skills/reasoning/refactor-prompt.md | 创建 opencode 原生 skill 与 slash command（/wiki-ingest、/wiki-refactor），更新 README 和 AGENTS 的 opencode 集成文档 | .opencode/skills/wiki-ingest/SKILL.md, .opencode/skills/wiki-refactor/SKILL.md, .opencode/command/wiki-ingest.md, .opencode/command/wiki-refactor.md, README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 支持 opencode 原生 `/` 命令和 skill 自动发现，降低手动提示词依赖 |
| 2026-07-01 | delete | .opencode/skills/wiki-ingest, .opencode/skills/wiki-refactor | 移除 opencode skill 文件，精简为纯 command 方案；command 改为自包含内容 | .opencode/command/wiki-ingest.md, .opencode/command/wiki-refactor.md, README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 简化架构，用户只需 command 即可，去除冗余的 skill 层 |
| 2026-07-01 | refactor | AGENTS.md 第六节、第九节 | 新增 /wiki-output 和 /wiki-check command，补全 raw→wiki→output 全链路 | .opencode/command/wiki-output.md, .opencode/command/wiki-check.md, README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 完善 command 矩阵，覆盖 ingest/refactor/output/check 四个核心流向 |
| 2026-07-01 | refactor | .opencode/command/wiki-ingest.md | 简化 /wiki-ingest 为纯自动发现模式，移除指定路径参数，放文件后直接 /wiki-ingest 即自动扫描并处理 | .opencode/command/wiki-ingest.md, README.md, wiki/lifecycle/operation-log.md | 进一步降低使用门槛 |
| 2026-07-01 | rename | .opencode/command/wiki-ingest.md, .opencode/command/wiki-refactor.md | 重命名：/wiki-ingest → /wiki-compile，/wiki-refactor → /wiki-update | .opencode/command/wiki-compile.md, .opencode/command/wiki-update.md, README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 命名更直观，compile=编译素材，update=更新结构 |
| 2026-07-01 | update | README.md | 在 README 中补充 opencode 专属 slash 命令的速查表与使用说明，并标注命令仅适用于 opencode | README.md, wiki/lifecycle/operation-log.md | 提升命令可发现性，明确使用环境 |
| 2026-07-01 | rename | .opencode/command/wiki-update.md | 重命名：/wiki-update → /wiki-refactor，消除"update"与版本更新/文件同步的歧义 | .opencode/command/wiki-refactor.md, README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 名称与 refactor 流程源对齐，语义更精确 |
| 2026-07-01 | delete | skills/ingest/, skills/reasoning/ | 彻底删除 skills/ 目录，将 ingest-prompt 的 Concept/Entity 模板与自检清单合并进 wiki-compile.md，将 refactor-prompt 的复利操作类型表、Systems/Comparisons 模板与生命周期记录格式合并进 wiki-refactor.md | .opencode/command/wiki-compile.md, .opencode/command/wiki-refactor.md, README.md, AGENTS.md, wiki/lifecycle/operation-log.md | 消除 command 与 skills 双源歧义，command 成为唯一自包含权威流程源 |
| 2026-07-01 | refactor | .opencode/command/ | 将 opencode 专属 command 迁移为跨工具 portable skills：删除 .opencode/ 目录，创建 skills/ 下 4 个纯 Markdown 工作流文件（去除 frontmatter 与 $ARGUMENTS）；新增 CLAUDE.md 作为 Claude Code 入口；AGENTS.md 第七节改为"工作流技能（跨工具）"；README 全面改版为跨工具说明 | skills/wiki-compile.md, skills/wiki-refactor.md, skills/wiki-output.md, skills/wiki-check.md, CLAUDE.md, AGENTS.md, README.md, wiki/lifecycle/operation-log.md | 基于 AGENTS.md 标准，使 Codex / Claude / opencode / Cursor 等工具均可直接使用，不再绑定 opencode |
