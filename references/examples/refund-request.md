# 信用服务退款

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `alipay.trade.refund` |
| 接口描述 | 扣款成功后发起退款 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/ce7e9000_alipay.trade.refund) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "alipay.trade.refund",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "trade_no": "2026041822001400001234567890",
    "refund_amount": "10.00",
    "out_request_no": "REFUND20260418001",
    "refund_reason": "用户申请退款"
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
| `refund_amount` | 是 | 退款金额，单位：元，精确到分 |
| `out_request_no` | 是 | 退款请求号，需保证唯一，用于幂等控制 |
| `refund_reason` | 否 | 退款原因 |

---

## 响应报文

### 完整示例

```json
{
  "alipay_trade_refund_response": {
    "code": "10000",
    "msg": "Success",
    "trade_no": "2026041822001400001234567890",
    "refund_fee": "10.00"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `trade_no` | 支付宝交易号 |
| `refund_fee` | 退款金额 |

---

## 使用说明

1. 退款需要使用扣款时返回的 `trade_no`（支付宝交易号）
2. 退款金额不能超过原扣款金额
3. 支持部分退款，可多次调用
4. **部分退款时 `out_request_no` 必填**，每次退款需传入不同的请求号
5. 退款成功后订单状态会变更为 `TRADE_CLOSED`