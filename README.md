# 芝麻先享 Skill

支付宝芝麻先享信用支付工具，支持服务开通、信用下单、扣款退款、订单完结等核心操作。

## 目录结构

```
├── skill/                    # 技能文档
│   ├── api-reference.md     # API 参考
│   ├── example-guide.md     # 示例校验
│   ├── faq.md               # 常见问题
│   ├── glossary.md          # 字段词典
│   ├── interaction-guide.md # 交互引导
│   └── workflow.md          # 业务流程
│
├── references/              # 参考资源
│   ├── config.json.example # 配置模板
│   └── examples/            # 示例报文
│
├── SKILL.md                 # 技能定义
└── zhima-credit-payafteruse.bundle.js  # 核心脚本
```

## 快速开始

```bash
# 1. 配置密钥
mkdir -p ~/.antConfig
cp references/config.json.example ~/.antConfig/config.json

# 2. 开通服务
node zhima-credit-payafteruse.bundle.js sign --out_agreement_no AGR001 --zm_service_id 您的服务ID

# 3. 创建订单
node zhima-credit-payafteruse.bundle.js order --credit_agreement_id 协议号 --out_order_no ORD001 --subject 标题
```

## 常用命令

| 命令 | 说明 |
|------|------|
| `sign` | 开通服务 |
| `query-agreement` | 查询协议 |
| `order` | 免密下单 |
| `create-order` | 确认下单 |
| `query` | 查询订单 |
| `pay` | 扣款 |
| `refund` | 退款 |
| `finish` | 结束订单 |
| `example` | 查看示例 |

## 文档导航

| 需求 | 查看 |
|------|------|
| 业务流程 | `skill/workflow.md` |
| 命令参数 | `skill/api-reference.md` |
| 常见问题 | `skill/faq.md` |
| 配置帮助 | `references/config.json.example` |

> 详细使用说明请参阅 [SKILL.md](./SKILL.md)