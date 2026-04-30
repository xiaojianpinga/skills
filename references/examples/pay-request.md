# 信用服务扣款

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `alipay.trade.pay` |
| 接口描述 | 信用订单扣款，基于信用订单号发起扣款 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/507b8b31_alipay.trade.pay) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "alipay.trade.pay",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "out_trade_no": "ZMCB99202604180000070000051245",
    "auth_code": "ZMCB99202604180000070000051245",
    "total_amount": "10.00",
    "subject": "快递服务费",
    "scene": "ZHIMA_CREDIT_CODE",
    "product_code": "GENERAL_WITHHOLDING",
    "is_async_pay": true,
    "extend_params": {
      "creditTradeScene": "CREDIT_PAY_UNCERTAIN_FEE"
    }
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
| `out_trade_no` | 是 | 商户订单号，使用信用订单号 |
| `auth_code` | 是 | 支付授权码，使用信用订单号 |
| `total_amount` | 是 | 扣款金额，单位：元，精确到分 |
| `subject` | 是 | 订单标题 |
| `scene` | 是 | 支付场景，固定值 `ZHIMA_CREDIT_CODE` |
| `product_code` | 是 | 产品码，固定值 `GENERAL_WITHHOLDING` |
| `is_async_pay` | 否 | 是否异步扣款，默认 `true` |
| `extend_params.creditTradeScene` | 是 | 信用交易场景，固定值 `CREDIT_PAY_UNCERTAIN_FEE` |

---

## 响应报文

### 完整示例

```json
{
  "alipay_trade_pay_response": {
    "code": "10000",
    "msg": "Success",
    "trade_no": "2026041822001400001234567890",
    "out_trade_no": "ZMCB99202604180000070000051245",
    "total_amount": "10.00",
    "buyer_logon_id": "138****5678"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `trade_no` | 支付宝交易号，用于后续退款或查询 |
| `out_trade_no` | 商户订单号 |
| `total_amount` | 交易金额 |
| `buyer_logon_id` | 买家支付宝账号（脱敏） |

---

## 使用说明

1. 信用订单扣款需要使用信用订单号（`credit_biz_order_id`）作为 `out_trade_no` 和 `auth_code`
2. `scene`、`product_code`、`extend_params.creditTradeScene` 为固定值，用于标识信用支付场景
3. 扣款成功后返回 `trade_no`，用于后续退款或交易查询
4. 异步扣款模式下，扣款结果需要通过交易查询接口确认