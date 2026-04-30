# 信用服务订单查询

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `zhima.credit.payafteruse.creditbizorder.query` |
| 接口描述 | 查询订单状态及信息 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/4b0b26e9_zhima.credit.payafteruse.creditbizorder.query) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditbizorder.query",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "credit_biz_order_id": "ZMCB99202604180000070000051245"
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
| `credit_biz_order_id` | 二选一 | 信用订单号，以 `ZMCB` 开头 |
| `out_order_no` | 二选一 | 商户订单号，8-64位 |

---

## 响应报文

### 完整示例

```json
{
  "zhima_credit_payafteruse_creditbizorder_query_response": {
    "code": "10000",
    "msg": "Success",
    "credit_biz_order_id": "ZMCB99202604180000070000051245",
    "out_order_no": "ORDER20260418001",
    "order_status": "INIT",
    "order_amount": "10.00"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `credit_biz_order_id` | 信用订单号 |
| `out_order_no` | 商户订单号 |
| `order_status` | 订单状态：`INIT`=初始化，`TRADE_CLOSED`=已关闭，`TRADE_FINISHED`=已完成 |
| `order_amount` | 订单金额 |

---

## 使用说明

1. 使用 `credit_biz_order_id` 或 `out_order_no` 查询（二选一）
2. 脚本会根据订单号格式自动判断类型（ZMCB开头为信用订单号）
3. 用户确认场景下单后，需调用此接口查询订单状态和获取 `credit_biz_order_id`