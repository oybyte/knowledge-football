---
schema_version: 1
document_id: DOC-REL-001
document_type: relation_registry
title: 规则关系登记表
domain_code: REL
retrieval_scope: active
track: formal
lifecycle: effective
version: 2.0.0
---

# 规则关系登记表

## 关系类型

| 类型 | 含义 | 执行方式 |
|---|---|---|
| requires | 先决条件 | 未满足不得调用后续规则 |
| supplements | 边界补充 | 在主规则之后检查，不复制权重 |
| branches | 条件分支 | 按场景互斥或择高档执行 |
| excludes | 禁用关系 | 命中后禁止另一规则 |
| conflicts | 同场景冲突 | 交由门禁裁决，不自动叠加 |
| observes | 实验观察 | 单独输出，不影响正式轨 |
| promotes | 转正关系 | 完成验证和两次确认后生效 |

## 核心关系

- `TRAP-001` requires `INPUT-001`、`LEAGUE-001`和基本面检查。
- `AP-003` branches `AP-004`，盘口共识不足时优先降级。
- `TRAP-002` excludes 具有明确基本面驱动的纯阻诱解释。
- `KELLY-002` excludes 胜平负单选输出，由`SCORE-003`执行表达门禁。
- `GOAL-003`、`GOAL-004`和`GOAL-007`按信号强度分支，不重复叠加。
- `EURO-004` conflicts 盘口方向明显背离时的强方向表达，由`GATE-004`降级。
- `LEAGUE-002` supplements 通用盘口规则，但杯赛、季后赛和友谊赛不直接套用。
- `LEAGUE-008`、`LEAGUE-009`、`LEAGUE-010` observes 正式轨结果，不改变正式轨权重。
- `GOAL-009`、`GOAL-010`和`KELLY-007` observes 正式轨对应维度。
- `GOAL-009` conflicts `GOAL-003`或`GOAL-004`时，由`GATE-006`记录实验分歧，不覆盖正式轨。
- `LEAGUE-008`、`LEAGUE-009`与正式轨对应规则的分歧，统一由`GATE-006`降级处理。
- 所有实验规则达到验证门槛后通过`promotes`转为正式轨，保留原规则ID。

## 冲突原则

先处理禁用条件，再按基本面、赛制、联赛专属、盘型、时间节点和交叉市场顺序裁决。无法裁决时回退通用规则，提升风险，不强行选择冲突方向。
