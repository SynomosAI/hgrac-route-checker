# hgrac-route-checker

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这类人类遗传资源活动要不要报、报哪条路」拆成可核验的属性，让 AI 只做路径提示，不做审批结论。

## 这个域管什么

人类遗传资源审批路径判定

人类遗传资源的采集、保藏、利用、对外提供分属不同审批路径，且涉及国家安全与生物安全。AI 能做的是把路径与要件摆清楚，审批权始终在行政部门。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `bio`（全局唯一，一经分配不复用） |
| 域名称 | 生物 · 人类遗传资源 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `bio.hgrac_activity` | enum | 活动类型，决定适用哪条审批路径 · 取值 collection/preservation/utilization/provision_abroad/open_access |
| `bio.hgrac_approval_status` | enum | 行政许可当前状态 · 取值 not_required/pending/approved/rejected/expired/unknown |
| `bio.entity_qualification` | enum | 中方单位资格与外方主体性质 · 取值 qualified/unqualified/foreign_entity/under_review |
| `bio.resource_type` | enum | 资源形态，影响合规要求强度 · 取值 organ/tissue/cell/blood/genetic_data/other |
| `bio.biosafety_level` | enum | 实验室生物安全等级 · 取值 bsl1/bsl2/bsl3/bsl4/not_applicable |

## 本域红线（不可逾越，机器可读）

1. 未取得行政许可的，不得输出可对外提供或开放使用的表述
2. 不得建议通过拆分申报、变更名目等方式规避审批
3. 不下审批通过结论，行政许可以审批部门书面决定为唯一依据

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据

- 人类遗传资源管理条例
- 人类遗传资源管理条例实施细则
- 中华人民共和国生物安全法

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
