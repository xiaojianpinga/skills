---
name: zhima-credit-payafteruse-skill
description: 芝麻先享智能操作助手，具备 Agent 自主执行能力。通过调用支付宝开放平台 API，可直接完成服务开通、信用下单、扣款退款、订单完结等业务操作，覆盖 9 个核心接口。支持免确认和需确认两种下单模式，适配不同业务场景。提供交互式引导与命令行两种使用方式，自动处理签名、生成二维码、查询协议状态。内置示例报文查询、参数校验、错误码排查等辅助功能，显著降低接入门槛。适用于开发者联调测试、运营快速操作、技术支持问题排查等场景。
author: yoojoy
origin: creditserviceskills
metadata:
  version: "1.28.0"
  created: "2026-03-24"
  updated: "2026-04-18"
  api: "zhima.credit.payafteruse.*"
---

# 芝麻先享Skill

## 📋 技能概述

本 Skill 提供芝麻先享（动态金额模式）的全流程操作能力，通过调用支付宝开放平台 API 实现信用服务的开通、下单、扣款及订单管理。

## 🔐 前置准备

### 申请条件

> ⚠️ **重要**：
> - 芝麻先享仅支持自研商家/服务商接入
> - 不支持沙箱调试，需使用生产环境

使用前请确保已完成：
- ✅ 在支付宝开放平台创建应用
- ✅ 申请芝麻先享产品能力（审核需1-3个工作日）
- ✅ 获取芝麻信用服务ID（应用详情页 → 能力列表，28位数字）

> 📖 详细接入流程请参阅 [支付宝开放平台文档](https://opendocs.alipay.com/open/03ulon)

### 配置文件

配置文件路径：`~/.antConfig/config.json`（用户主目录下）

必填配置项：
```json
{
  "X-OpenPlatform-alipayPublicKey": "<支付宝公钥>",
  "X-OpenPlatform-appId": "<应用 ID>",
  "X-OpenPlatform-PrivateKey": "<应用私钥>"
}
```

> 📖 配置示例请参阅 [config.json.example](references/config.json.example)

## 🚀 快速开始

详见 [README.md](./README.md)

> 📖 完整流程请参阅 [业务流程详解](skill/workflow.md)

---

## 💬 使用方式

本技能支持两种使用方式：

### 方式一：交互式引导

用户发起模糊请求时，通过交互式引导逐步完成操作：

```
用户：芝麻先享
助手：[自我介绍，让用户选择技术咨询或产品操作]
```

> 📖 详细引导规范请参阅 [交互引导规范](skill/interaction-guide.md)

### 方式二：直接指令

用户可通过明确指令直接调用，跳过交互引导：

| 命令 | 说明 | 示例 |
|------|------|------|
| `sign` | 开通服务 | `sign --out_agreement_no AGR001 --zm_service_id xxx` |
| `query-agreement` | 查询协议 | `query-agreement --credit_agreement_id ZMOP99xxx` |
| `order` | 免确认下单 | `order --credit_agreement_id xxx --out_order_no ORD001 --subject 标题` |
| `create-order` | 用户确认下单 | `create-order --zm_service_id xxx --out_order_no ORD001 --subject 标题` |
| `query` | 查询订单 | `query --order_id ZMCB99xxx` |
| `pay` | 扣款 | `pay --order_id ZMCB99xxx --total_amount 10.00 --subject 标题` |
| `query-trade` | 查询交易 | `query-trade --trade_no 20260410xxx` |
| `refund` | 退款 | `refund --trade_no 20260410xxx --refund_amount 10.00` |
| `finish` | 结束订单 | `finish --credit_biz_order_id ZMCB99xxx --is_fulfilled true` |
| `last-request` | 查看请求报文 | `last-request` |
| `last-result` | 查看响应报文 | `last-result` |
| `example` | 查询示例报文 | `example sign` |
| `validate` | 校验请求报文 | `validate sign，我的报文是...` |

> 💡 详细参数说明请参阅 [API 参考文档](skill/api-reference.md)

**技术咨询类指令：** `接入流程`、`接口文档`、`错误码`、`常见问题`、`模式区别`

## 🔄 业务流程

芝麻先享服务支持两种业务流程：

| 模式 | 核心特点 | 适用场景 |
|------|----------|----------|
| **免确认模式** | 首次开通确认后，后续可通过服务端调用完成下单 | 用户已开通服务，商家可免密下单；小金额、高频次场景 |
| **需确认模式** | 每次下单都需要用户确认 | 用户未开通或需用户主动确认下单；大金额、低频次场景 |

**免确认模式流程：**
```
1.开通服务 → 2.查询协议 → 3.免密下单 → 4.扣款 → 5.退款
                              │
                              └──→ 6.结束订单(不扣款)
```

**需确认模式流程：**
```
1.开通下单 → 2.扣款 → 3.退款
       │
       └──────────→ 4.结束订单(不扣款)
```

> 📖 详细的流程图及命令示例请参阅 [业务流程详解](skill/workflow.md)

## 📎 文档导航

详见 [README.md](./README.md) 或以下链接：

- [业务流程](skill/workflow.md) | [API 参考](skill/api-reference.md) | [常见问题](skill/faq.md)
- [示例报文](references/examples/) | [字段词典](skill/glossary.md) | [交互引导](skill/interaction-guide.md)

## ⚠️ 注意事项

1. **履约标识**：结束订单时 `is_fulfilled=true` 显示已守约，`false` 显示已取消
2. **免确认 vs 需确认**：
   - 免确认（`order`）：用户已开通，传入 `credit_agreement_id`
   - 需确认（`create-order`）：用户未开通，传入 `zm_service_id`
3. **自动处理**：`finish` 命令自动生成请求号，`pay` 命令自动使用信用订单号作为授权码

## 🐛 错误处理

| 错误码 | 错误信息 | 解决方案 |
|--------|----------|----------|
| `2000` | 参数错误 | 使用 `example` 查看示例报文，用 `validate` 校验参数 |
| `3000` | 服务不存在 | 确认服务ID正确（应用详情页 → 能力列表，28位数字） |
| `INVALID_AGREEMENT` | 协议无效 | 使用 `query-agreement` 查询协议状态 |
| `ORDER_NOT_FOUND` | 订单不存在 | 使用 `query` 查询订单状态 |
| `PAYMENT_FAILED` | 扣款失败 | 检查用户余额或扣款条件 |
| `CONFIG_ERROR` | 配置文件错误 | 检查配置文件路径和格式 |

> 📖 更多问题请参阅 [常见问题](skill/faq.md)

## 📝 更新日志

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v1.28.0 | 2026-04-18 | 重构文档结构，新增示例报文和评测体系 |

> 📖 [查看完整更新日志](CHANGELOG.md)

---

*最后更新：2026-04-18*