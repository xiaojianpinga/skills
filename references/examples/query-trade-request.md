# 交易查询

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `alipay.trade.query` |
| 接口描述 | 查询扣款交易结果 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/1bce7243_alipay.trade.query) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "alipay.trade.query",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "trade_no": "2026041822001400001234567890"
  },
  "sign": "签名值（根据私钥生成）"
}
```

### 字段说明

#### 公共参数

| 字段 | 必填 | 说明 |
|------|------|------|
| `app_id` | 是 | 支付宝应用ID |
| `method` | 是 | 接口名称 |
| `format` | 是 | 响应格式，固定值 `JSON` |
| `charset` | 是 | 字符集，固定值 `utf-8` |
| `sign_type` | 是 | 签名类型，固定值 `RSA2` |
| `timestamp` | 是 | 请求时间，格式 `yyyy-MM-dd HH:mm:ss` |
| `version` | 是 | 接口版本，固定值 `1.0` |
| `sign` | 是 | 签名值 |

#### 业务参数（biz_content）

| 字段 | 必填 | 说明 |
|------|------|------|
| `trade_no` | 是 | 支付宝交易号，扣款时返回的交易号 |

---

## 响应报文

### 完整示例

```json
{
  "alipay_trade_query_response": {
    "code": "10000",
    "msg": "Success",
    "trade_no": "2026041822001400001234567890",
    "out_trade_no": "ZMCB99202604180000070000051245",
    "total_amount": "10.00",
    "trade_status": "TRADE_SUCCESS",
    "buyer_logon_id": "138****5678",
    "buyer_user_id": "2088012336260399"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `trade_no` | 支付宝交易号 |
| `out_trade_no` | 商户订单号 |
| `total_amount` | 交易金额 |
| `trade_status` | 交易状态 |
| `buyer_logon_id` | 买家支付宝账号（脱敏） |
| `buyer_user_id` | 买家支付宝用户ID |

### 交易状态说明

| 状态 | 说明 |
|------|------|
| `WAIT_BUYER_PAY` | 交易创建，等待买家付款 |
| `TRADE_CLOSED` | 未付款交易超时关闭，或支付完成后全额退款 |
| `TRADE_SUCCESS` | 交易支付成功 |
| `TRADE_FINISHED` | 交易结束，不可退款 |

---

## 使用说明

1. 使用扣款时返回的 `trade_no` 查询交易状态
2. 异步扣款模式下，需通过此接口确认扣款结果
3. `TRADE_SUCCESS` 表示扣款成功，`TRADE_CLOSED` 表示交易关闭或已全额退款