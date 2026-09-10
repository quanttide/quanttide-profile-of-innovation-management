# 需求循环

https://github.com/hongwei-2026/product-requirement-loop

量潮产品需求梳理智能体（product-requirement Loop），量潮实训课题交付仓库，Python 实现。

## 定位

从产品研发日志中梳理需求的人机协作流水线，核心原则："电脑可以帮忙写，最后拍板的是人"。

## 核心流程

1. 日志库：从官方仓库或本机获取日志原文
2. AI Step1 整理初稿 → 人审（写理由，通过或打叉重跑，五种原因码）
3. AI Step2 抽取"用户要…" → 人审
4. 人点定稿归档 → 定稿档案可检索、默认不必重审

定稿落盘 `accepted.json`，含 `revisions` 与 `journal_id`，全程可溯源；审核历史留痕。

## 形态与边界

- CLI（`implementation.py` 官方入口，`loop_runner.py` 闭环编排）+ 本机 Web（`127.0.0.1:8765`，登录、日志库、工作台、定稿档案）
- 边界：不接飞书 API、不做 JSON→SQL；Web 为交付期"让同事用起来"的加餐，未推翻立项范围

## 验收门槛（立项 6 条硬门槛）

编造条数 = 0；`source_quote` 通过率 = 100%；句覆盖率 = 100%（例外写入 `uncovered.md` 人工确认）；反馈点 ≥ 1；`accepted.json` 每条 `approved: true` 且 `revisions` 非空；`check.py --strict` 退出码 0。实测报告见 `project/trials/case-01/trial-report.md`。

## 实训贡献机制

一账号同时只接 1 题：`/claim` 认领 → Design 落 `docs/designs/` → 维护者 Merge Design → `/accept @你` 后生效；Impl 须代码 + 视频 + 截图；关联 PR 超 30 天无更新自动释放。任务总览见 Issue #7（含积分制与接取人表）。

## 评估（2026-09-10）

过程工程复杂度与产品被验证程度严重不成比例，疑似为结项答辩而构建，而非为有人用起来而构建：

1. **流程机器远超产品体量**：仓库一周余、0 star、实际用例仅 1 个（case-01），却配了企业级治理：四步认领协议、积分制、30 天 SLA 自动释放、计分板、接取人状态 JSON；PR 须四人全部 Approve，对单人维护的本机工具是组织流程硬套个人项目。
2. **治理脚本比产品逻辑还多**：`scripts/` 下文档门禁、布局门禁、安全门禁、PR 范围守卫、计分同步全套齐活；维护成本落在维护者一人手工同步，长期必然腐化。
3. **包装篇幅远超产品迭代证据**：`docs/交付/` 下 17 个文档（答辩总报告、评审意见处理、汇报稿等），而核心 Loop 只有一条 trial 记录、一个演示账号。
4. **backlog 优先级倒置**：尚未有第二个真实用户，特难扩展 14 项却全是企业级 SaaS 课题（SLA 指派、SemVer 门禁、prompt_sha、多 trial 矩阵、JSON↔SQLite 单一数据源）。

## 数据与输出管线查证（2026-09-10）

项目设计时提供了两个量潮仓库作为输入/输出参照：

- **输入（quanttide-journal-of-product-development）：已接入**。`scripts/sync_journals.py` 从该仓库 GitHub API 拉取日志，catalog 同步 31 个产品流、63 篇日志；case-01 验收原文即 `qtcloud-product/2026-08-19.md`。
- **输出（quanttide-profile-of-product-development）：未接入**。它仅在结项报告/截图脚本中作为链接展示（`qtcloud-product/requirement.md`、`requirement.json` 被当作人工梳理成果的对照例子）；Loop 的定稿输出止步于本地 `accepted.json`，无任何导出或回流到档案仓库的代码路径。导出能力被排为 Issue #9（requirement.json 导出包）、#10（第二大脑 Context 导出包）两个待做的特难任务，且立项边界"不做 JSON→SQL"在立项时就划断了这条路。

结论：输入管线打通，输出管线断裂——定稿成果与量潮档案体系之间只有文档层面的示意对照，没有数据层面的连接。演示闭环做足了，真正接入量潮知识体系的一步（最有长期价值的一步）被排到了最低优先级。

辩护角度：实训课题的流程本身可能是教学内容（仿真企业协作），重流程说得通；但即使按教学标准，无自动化支撑的手工治理也难以长期维持。
