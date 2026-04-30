# 示例报文查询与校验指南

本文档介绍如何使用示例报文查询和请求报文校验功能，帮助开发者快速了解接口报文格式并自查报文正确性。

---

## 一、示例报文查询

### 1.1 使用方式

通过以下方式查询接口的完整示例报文：

```
用户：开通服务示例报文
用户：example sign
用户：扣款接口的示例
```

### 1.2 支持查询的接口

| 接口名称 | 关键词 |
|---------|--------|
| 开通服务 | sign、开通、creditagreement.sign |
| 查询协议 | query-agreement、查询协议、creditagreement.query |
| 免确认下单 | order、免确认下单、creditbizorder.order |
| 用户确认下单 | create-order、用户确认下单、creditbizorder.create |
| 查询订单 | query、查询订单、creditbizorder.query |
| 扣款 | pay、扣款、trade.pay |
| 结束订单 | finish、结束订单、creditbizorder.finish |
| 退款 | refund、退款、trade.refund |
| 交易查询 | query-trade、交易查询、trade.query |

### 1.3 返回内容

查询示例报文时，返回以下信息：

| 内容 | 说明 |
|------|------|
| 接口信息 | 接口名称、描述、请求方式、官方文档链接 |
| 请求报文示例 | 完整 JSON 格式，包含公共参数和业务参数 |
| 响应报文示例 | 完整 JSON 格式，包含所有响应字段 |
| 字段说明 | 必填标识、字段含义、取值范围 |
| 使用说明 | 接口调用注意事项 |

### 1.4 示例文件列表

所有示例报文文件位于 `references/examples/` 目录：

| 文件 | 接口 |
|------|------|
| [sign-request.md](examples/sign-request.md) | 信用服务开通/授权 |
| [query-agreement-request.md](examples/query-agreement-request.md) | 查询服务开通信息 |
| [order-request.md](examples/order-request.md) | 信用服务下单（免确认） |
| [create-order-request.md](examples/create-order-request.md) | 信用服务下单（用户确认） |
| [query-request.md](examples/query-request.md) | 信用服务订单查询 |
| [pay-request.md](examples/pay-request.md) | 信用服务扣款 |
| [finish-request.md](examples/finish-request.md) | 结束信用服务订单 |
| [refund-request.md](examples/refund-request.md) | 信用服务退款 |
| [query-trade-request.md](examples/query-trade-request.md) | 交易查询 |

---

## 二、请求报文校验

### 2.1 使用方式

提供您的请求报文，系统会与标准示例对比，检查必传字段和固定字段：

```
用户：校验报文 sign，我的报文是：
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditagreement.sign",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "out_agreement_no": "AGR001"
  }
}
```

### 2.2 校验内容

| 检查项 | 说明 |
|--------|------|
| 必传字段 | 检查所有必传字段是否已填写 |
| 固定字段值 | 检查固定值字段是否正确（如 method、format、charset、sign_type、version） |
| 业务固定字段 | 检查业务参数中的固定值（如扣款接口的 product_code、scene） |
| 字段格式 | 检查字段格式是否正确（如时间格式、金额格式） |

### 2.3 各接口字段说明

各接口的必传字段、固定字段详情请查阅对应的示例报文文件：

| 接口 | 示例报文 |
|------|----------|
| 开通服务 | [sign-request.md](examples/sign-request.md) |
| 查询协议 | [query-agreement-request.md](examples/query-agreement-request.md) |
| 免确认下单 | [order-request.md](examples/order-request.md) |
| 用户确认下单 | [create-order-request.md](examples/create-order-request.md) |
| 查询订单 | [query-request.md](examples/query-request.md) |
| 扣款 | [pay-request.md](examples/pay-request.md) |
| 结束订单 | [finish-request.md](examples/finish-request.md) |
| 退款 | [refund-request.md](examples/refund-request.md) |
| 交易查询 | [query-trade-request.md](examples/query-trade-request.md) |

> 💡 示例报文文件包含完整的请求/响应报文、字段说明（必填标识）、使用说明。

### 2.4 校验结果输出格式

```
════════════════════════════════════════════════════════════════
      请求报文校验结果
════════════════════════════════════════════════════════════════

接口：zhima.credit.payafteruse.creditagreement.sign（开通服务）

❌ 发现 2 个问题：

【缺失必传字段】
  • zm_service_id - 芝麻信用服务ID（28位数字）

【固定字段值错误】
  • format: 当前值 "XML"，应为 "JSON"
  • charset: 当前值 "gbk"，应为 "utf-8"

✅ 已正确填写的字段：
  • app_id: 2021xxxxxxxxxxxxxxx
  • method: zhima.credit.payafteruse.creditagreement.sign
  • out_agreement_no: AGR001

════════════════════════════════════════════════════════════════
```

---

## 三、常见问题

### Q1: 示例报文中的 app_id、sign 等字段如何填写？

示例报文中的 `app_id`、`sign` 等字段为示例值，实际调用时需要替换为您自己的配置：
- `app_id`：您的支付宝应用ID
- `sign`：根据您的应用私钥生成的签名值
- `timestamp`：当前请求时间

### Q2: 校验时提示固定字段值错误，但我没有传这个字段？

某些字段有固定值要求，即使您没有显式传值，SDK 或系统也会自动填充。如果校验提示错误，请检查您的 SDK 版本或手动添加该字段。

### Q3: 扣款接口的固定字段在哪里配置？

扣款接口的 `product_code`、`scene`、`extend_params.creditTradeScene` 是芝麻先享产品的固定配置，不需要修改。校验时会检查这些字段是否正确传递。

---

## 四、相关文档

- [API 参考文档](api-reference.md) - 接口列表、命令格式、使用示例
- [业务流程详解](workflow.md) - 免确认/需确认模式流程图
- [字段词典](glossary.md) - 字段含义、来源、使用场景