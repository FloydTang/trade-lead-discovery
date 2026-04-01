# 客户搜索 / 线索发现 Skill for OpenClaw

当前状态：可演示

这个目录提供 OpenClaw-native 的单节点包装版本，目标不是单独造一套飞书工作容器，而是把这个节点稳定挂到总链路里。

角色定位：

- `stage_worker`
- `客户搜索员`

当前职责：

- 输入搜索意图和约束
- 调用核心脚本生成候选名单
- 输出给下游线索整理使用

## 安装归口说明

当前仓库采用两层结构：

- 根目录 README：公开最小可用说明
- 当前目录：龙虾 / OpenClaw 的单节点运行说明
- 这个单节点仓库本身也保留可独立执行的最小功能

如果你要把这个节点装进龙虾，优先直接复制增强执行词给龙虾：

- [飞书增强入口：复制增强执行词给龙虾](https://evenbetter.feishu.cn/wiki/ADmiwiultihx6Yk1p2UcjfmVn6d)

如果链接打开失败，请使用登记在半斤九两群里的飞书账号打开。

如果你拿到的是半斤九两科技沟通过的执行包用户链接，也可以优先使用那个链接打开。

如果你当前还没有半斤九两科技的账号，需要联系半斤九两科技，请访问：[evenbetter.tech](https://evenbetter.tech)

仓库内源码基线：

- `../references/00-单节点增强执行词.md`
- `./SKILL.md`

## Feishu 接入约束

当前这个 OpenClaw 变体如果要接飞书，默认必须挂到同一个主 Base 下运行。

固定要求：

- 先查主 Base，再查 `Lead Workflow Master`
- 搜索结果默认写入同一个 Base 下的 `Lead Discovery Results`
- 不因为搜索阶段单独运行就新建一个新的多维表格
- 后续进入线索整理或其他节点时，继续复用原主记录和原工作容器
- 当前角色固定为 `stage_worker`
- 当前节点固定 `attach_only`
- `feishu_container_creation = forbidden`

统一目标：

- 所有数据最终统一挂到 `Trade Lead Workflow Hub`
- 单节点不独立声明飞书工作容器

## 推荐模型

- `coze/doubao-seed-2-0-mini-260215`

## 会员增强价值

增强层重点不是换业务逻辑，而是减少安装试错：

- 给龙虾一个可以直接复制的单节点执行词
- 明确这个节点只能做公开线索发现
- 明确它只能回挂 `Lead Discovery Results`
- 明确失败后要把结果交回主代理统一回写

推荐先读：

- `../references/00-单节点增强执行词.md`
- `./SKILL.md`

## 快速运行

```bash
python3 ./scripts/build_lead_discovery_from_openclaw.py \
  --input-json ./examples/sample-input.json
```

## 作者

半斤九两科技
