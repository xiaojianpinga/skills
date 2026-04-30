# 信用服务下单（用户确认场景）

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `zhima.credit.payafteruse.creditbizorder.create` |
| 接口描述 | 用户确认下单，适用于用户未开通或需主动确认的场景 |
| 请求方式 | sdkExecute（生成签名字符串，由客户端唤起支付宝） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/dfb97313_zhima.credit.payafteruse.creditbizorder.create) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditbizorder.create",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "zm_service_id": "2022111500000000000093666900",
    "out_order_no": "ORDER20260418002",
    "subject": "快递服务费",
    "out_agreement_no": "AGR20260418002",
    "order_amount": "10.00",
    "cancel_back_link": "https://example.com/cancel",
    "return_back_link": "https://example.com/return"
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
| `zm_service_id` | 是 | 芝麻信用服务ID，28位数字 |
| `out_order_no` | 是 | 商户订单号，商户需保证唯一 |
| `subject` | 是 | 订单标题 |
| `out_agreement_no` | 否 | 商户外部协议号 |
| `order_amount` | 否 | 订单金额，动态金额模式可不传 |
| `cancel_back_link` | 否 | 取消回跳地址 |
| `return_back_link` | 否 | 完成回跳地址 |

---

## 响应报文

### 完整示例

```json
{
  "signStr": "app_id=2021xxxxxxxxxxxxxxx&biz_content=%7B%22zm_service_id%22%3A%22...&sign=xxx",
  "schemeUrl": "alipays://platformapi/startapp?appId=20000067&url=..."
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `signStr` | 签名字符串，用于客户端唤起支付宝 |
| `schemeUrl` | 完整的 scheme 链接，可直接在浏览器打开唤起支付宝 |

---

## 使用说明

1. 此接口用于**需确认模式**，用户可能未开通服务
2. 服务端生成 `signStr` 和 `schemeUrl`，客户端使用唤起支付宝
3. 用户在支付宝中确认开通并下单（一步完成）
4. 下单成功后可通过订单查询接口获取 `credit_biz_order_id`