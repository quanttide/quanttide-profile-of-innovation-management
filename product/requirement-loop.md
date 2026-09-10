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

一账号同时只接 1 题：`/claim` 认领 → Design 落 `docs/designs/` → 维护者 Merge Design → `/accept @你` 后生效；Impl 须代码 + 视频 + 截图；关联 PR 超 30 天无更新自动释放。
