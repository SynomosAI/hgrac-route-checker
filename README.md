# hgrac-route-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-bio%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Domain: HGRAC](https://img.shields.io/badge/domain-human%20genetic%20resource-8a2be2.svg)](domain.manifest.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这类人类遗传资源活动要不要报、报哪条路」拆成可核验的属性，让 AI 只做路径提示，不做审批结论。

**关键词 / Keywords**：人类遗传资源 · 采集保藏利用对外提供 · 审批路径 · 生物安全 · 行政许可 · HGRAC · domain profile · compliance checker · regulatory verification · human genetic resources · biosafety · approval routing · MCP tool · attribute verification · gate map · privacy by default-off

---

## 这个域管什么 / What this domain covers

人类遗传资源审批路径判定

人类遗传资源的采集、保藏、利用、对外提供分属不同审批路径，且涉及国家安全与生物安全。AI 能做的是把路径与要件摆清楚，审批权始终在行政部门。

This domain is the **bio · human genetic resources (HGRAC)** profile of a multi-domain verification platform. It turns "does this human-genetic-resource activity need filing, and which route" into verifiable attributes; the AI only surfaces the route and requirements — approval stays with the administrative authority.

---

## 快速开始 / Quick Start

> 本仓是**域声明骨架**（行业接入四件套 JSON），不是可运行的应用程序。克隆后用标准 JSON 工具即可校验结构。

```bash
# 1. 克隆本仓 / clone this repo
git clone https://github.com/SynomosAI/hgrac-route-checker.git
cd hgrac-route-checker

# 2. 校验四件套结构是否为合法 JSON（任意一个报错即骨架不完整）
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json     > /dev/null

# 3. 查看本域名称与红线 / print domain name and red lines
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));print(m['domain_name']);[print('-',r) for r in m['red_lines']]"
```

核验器（MCP 工具）的输入输出契约见 `checker.io.json`。一个最小调用示例：

```json
{
  "subject_ref": "mpp:cn:5e2a9b7c",
  "attribute_keys": ["bio.hgrac_activity", "bio.hgrac_approval_status"],
  "evidence": ["tsa:2026-09-12#sha256:c3d4..."],
  "request_id": "req-20260912-0007"
}
```

> 核验器只有「出示与核验」一个动作，**没有「判定」选项**——`已获批可对外提供`、`可开放使用`、`审批通过`等面向客户的结论性输出均在 `checker.io.json` 的 `_forbidden_outputs` 黑名单中。

---

## 域标识 / Domain identity

| 项 | 值 |
|---|---|
| 域 ID | `bio`（全局唯一，一经分配不复用） |
| 域名称 | 生物 · 人类遗传资源 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单 / Attributes

| 属性键 | 类型 | 说明 |
|---|---|---|
| `bio.hgrac_activity` | enum | 活动类型，决定适用哪条审批路径 · 取值 collection/preservation/utilization/provision_abroad/open_access |
| `bio.hgrac_approval_status` | enum | 行政许可当前状态 · 取值 not_required/pending/approved/rejected/expired/unknown |
| `bio.entity_qualification` | enum | 中方单位资格与外方主体性质 · 取值 qualified/unqualified/foreign_entity/under_review |
| `bio.resource_type` | enum | 资源形态，影响合规要求强度 · 取值 organ/tissue/cell/blood/genetic_data/other |
| `bio.biosafety_level` | enum | 实验室生物安全等级 · 取值 bsl1/bsl2/bsl3/bsl4/not_applicable |

## 本域红线（不可逾越，机器可读）/ Red lines

1. 未取得行政许可的，不得输出可对外提供或开放使用的表述
2. 不得建议通过拆分申报、变更名目等方式规避审批
3. 不下审批通过结论，行政许可以审批部门书面决定为唯一依据

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套 / The four-piece access set

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker.io.json` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 仓库结构 / Repository layout

```text
hgrac-route-checker/
├── README.md              # 本文件（中文为主，英文对照）
├── README.en.md           # English README
├── LICENSE                # MIT License（© 2026 SynomosAI）
├── CODEOWNERS             # @SynomosAI/synomosai-gov
├── domain.manifest.json  # 本域声明：属性清单 / 签发方 / 有效期 / 红线
├── gate-map.json          # 本域摩擦分级：silent/warn/confirm/block/require-owner
├── privacy.json           # 本域隐私声明：默认关闭 / 最小必要 / 可撤回 / 可删除
├── checker.io.json         # 本域核验器（MCP 工具）IO 契约
├── llms.txt               # 给 AI 的最小索引（llms.txt 标准）
├── llms-full.txt          # 给 AI 的完整文本
├── AGENTS.md              # 给 AI Agent 的工作指引
└── SECURITY.md            # 安全策略与报告途径
```

## 核心原则 / Core principles

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据 / References

- 人类遗传资源管理条例
- 人类遗传资源管理条例实施细则
- 中华人民共和国生物安全法

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式 / How to claim

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

---

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
