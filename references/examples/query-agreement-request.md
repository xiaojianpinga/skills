# 查询服务开通/授权信息

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `zhima.credit.payafteruse.creditagreement.query` |
| 接口描述 | 查询用户开通状态及相关信息 |
| 请求方式 | HTTP POST（JSON响应） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/999ae950_zhima.credit.payafteruse.creditagreement.query) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditagreement.query",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "credit_agreement_id": "ZMOP99202604120200390051554561"
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
| `credit_agreement_id` | 二选一 | 开通协议号，用户开通服务后获取 |
| `out_agreement_no` | 二选一 | 商户外部协议号 |

---

## 响应报文

### 完整示例

```json
{
  "zhima_credit_payafteruse_creditagreement_query_response": {
    "code": "10000",
    "msg": "Success",
    "credit_agreement_id": "ZMOP99202604120200390051554561",
    "out_agreement_no": "AGR20260418001",
    "agreement_status": "VALID",
    "zm_service_id": "2022111500000000000093666900",
    "alipay_user_id": "2088012336260399",
    "biz_time": "2026-04-18 10:30:00"
  }
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `code` | 响应码，`10000` 表示成功 |
| `msg` | 响应消息 |
| `credit_agreement_id` | 开通协议号 |
| `out_agreement_no` | 商户外部协议号 |
| `agreement_status` | 协议状态：`VALID`=有效，`INVALID`=无效，`UNSIGNED`=未签约 |
| `zm_service_id` | 芝麻信用服务ID |
| `alipay_user_id` | 支付宝用户ID |
| `biz_time` | 业务时间 |

---

## 使用说明

1. 使用 `credit_agreement_id` 或 `out_agreement_no` 查询（二选一）
2. 推荐使用 `credit_agreement_id` 查询，更准确
3. 开通成功后 `agreement_status` 为 `VALID`