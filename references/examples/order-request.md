# 信用服务下单（免用户确认场景）

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `zhima.credit.payafteruse.creditbizorder.order` |
| 接口描述 | 用户已开通服务，服务端直接下单，无需用户确认 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/144e086e_zhima.credit.payafteruse.creditbizorder.order) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditbizorder.order",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "credit_agreement_id": "ZMOP99202604120200390051554561",
    "out_order_no": "ORDER20260418001",
    "order_title": "快递服务费",
    "subject": "快递服务费",
    "order_amount": "10.00",
    "expire_time": "2026-04-25 10:30:00"
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
| `credit_agreement_id` | 是 | 开通协议号，用户开通服务后获取 |
| `out_order_no` | 是 | 商户订单号，商户需保证唯一，8-64位 |
| `order_title` | 是 | 订单标题 |
| `subject` | 是 | 订单标题（同时传 order_title 和 subject） |
| `order_amount` | 否 | 订单金额，动态金额模式可不传 |
| `expire_time` | 否 | 订单过期时间，格式 `yyyy-MM-dd HH:mm:ss` |

---

## 响应报文

### 完整示例

```json
{
  "zhima_credit_payafteruse_creditbizorder_order_response": {
    "code": "10000",
    "msg": "Success",
    "credit_biz_order_id": "ZMCB99202604180000070000051245",
    "out_order_no": "ORDER20260418001",
    "order_status": "INIT"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `credit_biz_order_id` | 信用订单号，以 `ZMCB` 开头 |
| `out_order_no` | 商户订单号 |
| `order_status` | 订单状态：`INIT`=初始化，`TRADE_CLOSED`=已关闭，`TRADE_FINISHED`=已完成 |

---

## 使用说明

1. 此接口用于**免确认模式**，用户已开通服务后直接下单
2. 需要传入用户开通时获取的 `credit_agreement_id`
3. 下单成功后返回 `credit_biz_order_id`，用于后续扣款或完结
4. 动态金额模式下 `order_amount` 可不传，扣款时再指定金额