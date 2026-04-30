# 芝麻先享字段词典

> 本文档整理芝麻先享服务涉及的关键字段，便于快速检索和理解。

---

## 核心字段速查

| 字段 | 中文名 | 来源 | 使用命令 | 说明 |
|------|--------|------|----------|------|
| `zm_service_id` | 芝麻信用服务ID | 芝麻运营分配 | sign, create-order | 28位，商户接入时获取 |
| `out_agreement_no` | 商户外部协议号 | 商户生成 | sign, query-agreement | 商户侧生成，不可重复 |
| `credit_agreement_id` | 开通协议号 | sign 返回 | query-agreement, order | 芝麻侧生成，用户开通后获取 |
| `out_order_no` | 商户订单号 | 商户生成 | order, create-order, query | 商户侧生成，8-64位 |
| `credit_biz_order_id` | 信用订单号 | order 返回 | query, pay, finish | 芝麻侧生成，每笔下单生成一个 |
| `trade_no` | 支付宝交易号 | pay 返回 | query-trade, refund | 支付宝生成，每次扣款生成一个 |

---

## 字段详解

### 1. zm_service_id（芝麻信用服务ID）

| 属性 | 说明 |
|------|------|
| **中文名** | 芝麻信用服务ID |
| **格式** | 28位数字 |
| **来源** | 芝麻运营分配，需申请接入 |
| **生成时机** | 商户接入审核通过后 |
| **使用命令** | `sign`、`create-order` |
| **使用场景** | 开通服务、用户确认场景下单 |

**示例**：`2022111500000000000093666900`

---

### 2. out_agreement_no（商户外部协议号）

| 属性 | 说明 |
|------|------|
| **中文名** | 商户外部协议号 |
| **格式** | 字母数字组合，最长64位 |
| **来源** | 商户侧生成 |
| **生成时机** | 调用开通接口前 |
| **使用命令** | `sign`、`query-agreement` |
| **使用场景** | 开通服务、查询协议 |

**约束**：
- 不同用户需传不同值
- 不可重复

**示例**：`AGR20260410001`

---

### 3. credit_agreement_id（开通协议号）

| 属性 | 说明 |
|------|------|
| **中文名** | 开通协议号 |
| **格式** | 以 `ZM` 或 `ZMOP` 开头 |
| **来源** | 芝麻侧生成（开通成功后返回） |
| **生成时机** | 用户确认开通后 |
| **使用命令** | `query-agreement`、`order` |
| **使用场景** | 查询协议、免确认场景下单 |

**约束**：
- 同一用户在同一商户下只有一个
- 用户关闭服务后重新开通会更新

**示例**：`ZMOP99202604030200810014698363`

**与 out_agreement_no 关系**：
- 多个 `out_agreement_no` 可对应同一个 `credit_agreement_id`
- 用户多次开通返回同一个 `credit_agreement_id`

---

### 4. out_order_no（商户订单号）

| 属性 | 说明 |
|------|------|
| **中文名** | 商户订单号 |
| **格式** | 8-64位字母、数字、下划线或横线组合 |
| **来源** | 商户侧生成 |
| **生成时机** | 调用下单接口前 |
| **使用命令** | `order`、`create-order`、`query` |
| **使用场景** | 创建订单、查询订单 |

**约束**：
- 需保证唯一性

**示例**：`ORDER20260410001`

---

### 5. credit_biz_order_id（信用订单号）

| 属性 | 说明 |
|------|------|
| **中文名** | 信用订单号 |
| **格式** | 以 `ZMCB` 开头，后跟20-30位数字 |
| **来源** | 芝麻侧生成（下单成功后返回） |
| **生成时机** | 创建订单成功后 |
| **使用命令** | `query`、`pay`、`finish` |
| **使用场景** | 查询订单、扣款、完结订单 |

**约束**：
- 一个信用订单只能扣款一次
- 扣款成功后自动完结

**示例**：`ZMCB99202604070000810078266187`

---

### 6. trade_no（支付宝交易号）

| 属性 | 说明 |
|------|------|
| **中文名** | 支付宝交易号 |
| **格式** | 20-32位纯数字 |
| **来源** | 支付宝生成（扣款成功后返回） |
| **生成时机** | 扣款成功后 |
| **使用命令** | `query-trade`、`refund` |
| **使用场景** | 查询交易、退款 |

**示例**：`2026041022001400001234567890`

---

## 字段流转关系图

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           字段流转关系                                        │
└─────────────────────────────────────────────────────────────────────────────┘

  商户生成                    芝麻生成                      支付宝生成
  ────────                   ────────                      ────────
     │                          │                             │
     ▼                          ▼                             ▼
┌──────────────┐         ┌──────────────┐              ┌──────────────┐
│out_agreement │────────▶│credit_       │              │trade_no      │
│_no           │  1:N    │agreement_id  │              │              │
└──────────────┘         └──────────────┘              └──────────────┘
                                │                             ▲
                                │ 1:N                         │ 1:1
                                ▼                             │
                         ┌──────────────┐              ┌──────────────┐
                         │credit_biz_   │─────────────▶│pay命令       │
                         │order_id      │   扣款成功    │返回          │
                         └──────────────┘              └──────────────┘
                                ▲
                                │ 1:1
                         ┌──────────────┐
                         │out_order_no  │
                         │(商户生成)     │
                         └──────────────┘

  zm_service_id（芝麻分配，全局使用）
  ─────────────────────────────────────
```

---

## 字段数量约束

| 约束规则 | 说明 |
|----------|------|
| 一用户一服务 | 同一用户在同一商户下只有一个 `credit_agreement_id` |
| 一服务多订单 | 一个 `credit_agreement_id` 可生成多个 `credit_biz_order_id` |
| 一订单一扣款 | 一个 `credit_biz_order_id` 只能扣款一次，生成一个 `trade_no` |

---

## 命令-字段对照表

| 命令 | 输入字段 | 输出字段 |
|------|----------|----------|
| `sign` | out_agreement_no, zm_service_id, category_id?, cancel_back_link?, return_back_link?, extra_param? | schemeUrl（用户确认后获得 credit_agreement_id） |
| `query-agreement` | credit_agreement_id 或 out_agreement_no | credit_agreement_id, agreement_status |
| `order` | credit_agreement_id, out_order_no, order_title, subject, order_amount?, expire_time? | credit_biz_order_id, order_status |
| `create-order` | zm_service_id, out_order_no, subject, out_agreement_no?, order_amount?, cancel_back_link?, return_back_link? | schemeUrl（用户确认后获得 credit_biz_order_id） |
| `query` | credit_biz_order_id 或 out_order_no | credit_biz_order_id, order_status |
| `pay` | out_trade_no, auth_code, total_amount, subject, is_async_pay? | trade_no, total_amount |
| `query-trade` | trade_no | trade_no, trade_status |
| `refund` | trade_no, refund_amount, refund_reason? | refund_fee |
| `finish` | credit_biz_order_id, is_fulfilled, extra_param.cancelReason?, out_request_no(自动) | 完结结果 |

> **说明**：`?` 表示可选字段，`(自动)` 表示脚本自动生成的字段。

### pay 命令硬编码字段

| 字段 | 值 | 说明 |
|------|-----|------|
| `product_code` | `GENERAL_WITHHOLDING` | 产品码 |
| `scene` | `ZHIMA_CREDIT_CODE` | 支付场景 |
| `extend_params.creditTradeScene` | `CREDIT_PAY_UNCERTAIN_FEE` | 信用交易场景 |