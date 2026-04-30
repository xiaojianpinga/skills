# 结束信用服务订单

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `zhima.credit.payafteruse.creditbizorder.finish` |
| 接口描述 | 完结或取消信用服务订单 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/31ea8bd1_zhima.credit.payafteruse.creditbizorder.finish) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditbizorder.finish",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "credit_biz_order_id": "ZMCB99202604180000070000051245",
    "is_fulfilled": "true",
    "out_request_no": "REQ20260418001"
  },
  "sign": "签名值（根据私钥生成）"
}
```

### 取消订单示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditbizorder.finish",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "credit_biz_order_id": "ZMCB99202604180000070000051245",
    "is_fulfilled": "false",
    "out_request_no": "REQ20260418002",
    "extra_param": "{\"cancelReason\":\"用户取消订单\"}"
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
| `credit_biz_order_id` | 是 | 信用订单号，以 `ZMCB` 开头 |
| `is_fulfilled` | 是 | 是否履约：`"true"`=已守约，`"false"`=已取消（字符串类型） |
| `out_request_no` | 是 | 商户请求号，需保证唯一 |
| `extra_param` | 否 | 扩展参数，JSON字符串格式，可包含 `cancelReason` |

---

## 响应报文

### 完整示例

```json
{
  "zhima_credit_payafteruse_creditbizorder_finish_response": {
    "code": "10000",
    "msg": "Success",
    "credit_biz_order_id": "ZMCB99202604180000070000051245",
    "out_request_no": "REQ20260418001"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `credit_biz_order_id` | 信用订单号 |
| `out_request_no` | 商户请求号 |

---

## 使用说明

1. `is_fulfilled` 为字符串类型：`"true"` 表示订单已履约（守约），`"false"` 表示订单取消
2. 取消订单时可在 `extra_param` 中传入 `cancelReason` 说明取消原因
3. `out_request_no` 需保证唯一，用于幂等控制
4. 订单完结后不可再进行扣款操作