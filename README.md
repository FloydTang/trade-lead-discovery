# Trade Lead Discovery

用公开网页和 LinkedIn 结果线索，找出第一批可继续进入线索整理的候选客户。

An open-source Codex skill for discovering the first batch of public-web prospect companies and turning them into a structured lead list ready for screening.

当前状态：可交付

角色定位：`客户搜索员`

这个仓库的开源内容本身即可独立使用，飞书增强入口不是必需安装步骤，而是增强体验选项。

链路角色：

- 在总链路里是 `stage_worker`
- 组合包 / 主代理是 `workflow_owner`
- 单节点默认 `attach_only`
- `feishu_container_creation = forbidden`
- 单节点不独立声明飞书工作容器
- 所有数据最终统一挂到同一个 `Trade Lead Workflow Hub`

上下游关系：

- 上游：产品、市场、客户类型、关键词 brief
- 下游：[trade-lead-screening](https://github.com/FloydTang/trade-lead-screening)

## 公开最小可用说明

这个仓库公开层只解决一个问题：

- 给你一个能单独跑通的客户搜索最小版本
- 和组合包一样，这个单节点仓库本身就拥有可独立执行的最小功能

## 两种权益

这个 Skill 当前分成两种使用权益：

### 1. 开源权益

- 直接使用当前 GitHub 仓库里的开源内容
- 即插即用，适合先跑通最小版本
- 适合自己阅读 README、运行脚本、替换样例和继续二次改造

### 2. 增强权益

- 在开源最小能力基础上，额外使用半斤九两科技提供的飞书增强执行词
- 更适合龙虾 / OpenClaw 安装和执行
- 使用体验会更精致、更完整，理解障碍和安装试错更少
- 更容易和统一的 `Trade Lead Workflow Hub` 挂接

当前最小能力：

- 生成固定搜索查询
- 搜公开网页结果和 LinkedIn 公司结果线索
- 去重并整理成结构化候选名单
- 输出来源链接、可见联系人线索和补查建议
- 桥接成 `trade-lead-screening` 输入

## 飞书增强入口

这个 Skill 的开源版本身就可以单独使用，并能完成当前节点的最小可用功能。

如果你希望在龙虾 / OpenClaw 中获得更精致、更完整的使用体验，建议按下面流程复制增强执行词：

- [飞书增强入口：复制增强执行词给龙虾](https://evenbetter.feishu.cn/wiki/ADmiwiultihx6Yk1p2UcjfmVn6d)

如果链接打不开，请先确认使用和半斤九两科技会员群绑定的飞书账号登录。

如果你暂时还没有绑定过，或当前还没有半斤九两科技的账号，请访问：[evenbetter.tech](https://evenbetter.tech)

仓库内对应的源码基线在：

- `references/00-单节点增强执行词.md`
- `for-openclaw/README.md`
- `for-openclaw/SKILL.md`

## 推荐模型

- `coze/doubao-seed-2-0-mini-260215`

## 这个仓库适合谁

- 知道产品、市场和客户类型，但不会系统搜客户的人
- 想快速做出一批公开候选名单的人
- 想搭建 `客户搜索 -> 线索整理 -> 客户背调 -> 开发信` 链路的人

## Quick Start

```bash
python3 ./scripts/build_lead_discovery_report.py \
  --input-json ./examples/frozen-food-search.json \
  --markdown-out /tmp/lead-discovery.md \
  --json-out /tmp/lead-discovery.json
```

```bash
python3 ./scripts/build_lead_screening_input.py \
  --input-json ./examples/frozen-food-output.json \
  --json-out /tmp/lead-screening-input.json
```

```bash
python3 ./scripts/run_regression_checks.py
```

```bash
python3 ./scripts/run_pre_release_gate.py
```

## Feishu / OpenClaw Stage Export

如果你要把这个 Skill 的输出直接接入飞书主表或搜索结果表，可在生成主输出后再运行：

```bash
python3 ./scripts/build_feishu_stage_payload.py \
  --input-json ./examples/frozen-food-output.json \
  --combo-run-id demo-run
```

这个脚本不会重新搜索。

它只负责把已有的 lead discovery 输出转换成 OpenClaw 可消费的阶段 payload，用于：

- 写入 `Lead Discovery Results`
- 回写 `Lead Workflow Master`
- 让后续线索整理或单点使用时仍能和主表融合

## Verification Status

当前版本包含：

- 固定样例输入输出
- 回归检查
- pre-release gate
- 最小 `for-openclaw/` 变体
- 与 `trade-lead-screening` 的桥接输出

真实联网验证情况：

- 当前脚本已验证到“联网搜索失败时不崩溃”
- 在当前环境下，真实搜索结果仍受 DuckDuckGo / 搜索源可用性影响，可能返回空候选
- 当前发布判断主要基于固定 fixtures、回归脚本和 gate，而不是实时搜索结果数量

## Chain Position

推荐链路：

`trade-lead-discovery -> trade-lead-screening -> trade-customer-intel -> trade-outreach-email`

关联仓库：

- 线索整理 Skill: [trade-lead-screening](https://github.com/FloydTang/trade-lead-screening)
- 客户背调 Skill: [trade-customer-intel](https://github.com/FloydTang/trade-customer-intel)
- 开发信 Skill: [trade-outreach-email](https://github.com/FloydTang/trade-outreach-email)

## Agent-First 增强价值

会员增强层当前不是改业务逻辑，而是补这几件事：

- 单节点在龙虾里有明确的 `stage_worker` 角色
- 单节点默认只 attach，不独立建飞书工作容器
- 飞书里提供可直接复制给龙虾的增强执行词
- 与总编排链路保持同一套回挂字段、失败回报和协作口径

## Repository Structure

```text
.
├── README.md
├── SKILL.md
├── 立项方案.md
├── 验收记录.md
├── scripts/
│   ├── build_lead_discovery_report.py
│   ├── build_lead_screening_input.py
│   ├── run_regression_checks.py
│   └── run_pre_release_gate.py
├── examples/
├── references/
│   ├── 00-单节点增强执行词.md
│   ├── input-fields.md
│   ├── lead-screening-integration.md
│   ├── output-template.md
│   └── search-rules.md
├── schemas/
└── for-openclaw/
```

## OpenClaw Variant

`for-openclaw/` 提供和总仓一致口径的单节点 OpenClaw 包装版本：

- 角色固定为 `stage_worker`
- 默认只允许 attach 到 `Trade Lead Workflow Hub`
- 不允许独立创建 Base、主表或平行工作容器

## 作者

半斤九两科技
