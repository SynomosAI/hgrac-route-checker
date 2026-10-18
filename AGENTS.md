# AGENTS.md — 给 AI Agent 的工作指引

本文件面向自动化 AI Agent（校验脚本、文档机器人、认领运营方）。人类读者请看 [README.md](README.md)。

## 这个仓库是什么

`hgrac-route-checker` 是一个 **domain profile（域声明）仓**，domain_id=`bio`，域名称「生物 · 人类遗传资源」。它不是可运行的应用程序：核心资产是四份机器可读 JSON（`domain.manifest.json` / `gate-map.json` / `privacy.json` / `checker.io.json`）。核验器是一个 MCP 工具契约（`checker.io.json`），**只有「出示与核验」一个动作，没有「判定」选项**。

## 可用的工作命令

```bash
# 克隆
git clone https://github.com/SynomosAI/hgrac-route-checker.git

# 校验四件套为合法 JSON
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json      > /dev/null

# 查看域名称与红线
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));print(m['domain_name'],m['status'])"
```

前置依赖：Python 3（仅用于本地 JSON 自检）；不需要安装任何第三方包。

## 约定（Do）

- 修改 `domain.manifest.json` 后，**必须同步**：`gate-map.json` 的红线覆盖、`privacy.json` 的属性清单、README 的「属性清单 / 本域红线」段与 `llms.txt` / `llms-full.txt`。
- 新增/改名属性键时，保持 `bio.` 前缀命名空间，并在 `domain.manifest.json` 的 `attributes` 与 `privacy.json` 的 `attr_keys` 两处一致。
- `bio.hgrac_approval_status` 与 `bio.hgrac_activity` 的取值改动须与 `gate-map.json` 中对外提供结论、开放使用声明的 block 规则保持一致。

## 禁止（Don't）

- 不要改动根目录 `LICENSE`、README「许可与署名」段的权属表述——权属字段只由维护者变更。
- 不要在 `checker.io.json` 的输出契约里加入「已获批可对外提供」「可开放使用」「审批通过」等面向客户的审批结论。
- 不要提交任何密钥、令牌或真实人遗资源样本 / 遗传数据；本域属性默认 `visibility_default=private`。
- 不要绕过组织 ruleset：main 分支禁止直推，任何改动走 Pull Request 并等待 lgd-ci-gate 通过。

## 边界

本域专注**人类遗传资源活动的审批路径（HGRAC）与生物安全**的属性化与核验契约；不覆盖具体实验技术或临床研究运营。核验输出为「出处链是否完整」，不构成行政许可或审批结论。
