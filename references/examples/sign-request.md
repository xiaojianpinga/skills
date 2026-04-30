# 信用服务开通/授权

## 接口信息

| 属性 | 值 |
|------|-----|
| 接口名称 | `zhima.credit.payafteruse.creditagreement.sign` |
| 接口描述 | 建立商家与用户关系，获取开通协议号 |
| 请求方式 | sdkExecute（生成签名字符串，由客户端唤起支付宝） |
| 官方文档 | [查看](https://opendocs.alipay.com/open/4580646b_zhima.credit.payafteruse.creditagreement.sign) |

---

## 请求报文

### 完整示例

```json
{
  "app_id": "2021xxxxxxxxxxxxxxx",
  "method": "zhima.credit.payafteruse.creditagreement.sign",
  "format": "JSON",
  "charset": "utf-8",
  "sign_type": "RSA2",
  "timestamp": "2026-04-18 10:30:00",
  "version": "1.0",
  "biz_content": {
    "out_agreement_no": "AGR20260418001",
    "zm_service_id": "2022111500000000000093666900",
    "cancel_back_link": "https://example.com/cancel",
    "return_back_link": "https://example.com/return",
    "extra_param": "{\"key\":\"value\"}"
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
| `out_agreement_no` | 是 | 商户外部协议号，商户需保证唯一，不同用户需传不同值 |
| `zm_service_id` | 是 | 芝麻信用服务ID，28位数字，由芝麻运营分配 |
| `category_id` | 否 | 外部类目，固定值 `ZHIMA_AUTH` |
| `cancel_back_link` | 否 | 取消回跳地址，用户取消开通后跳转的页面 |
| `return_back_link` | 否 | 完成回跳地址，用户完成开通后跳转的页面 |
| `extra_param` | 否 | 扩展参数，JSON字符串格式 |

---

## 响应报文

### 完整示例

```json
{
  "signStr": "app_id=2021xxxxxxxxxxxxxxx&biz_content=%7B%22out_agreement_no%22%3A%22AGR20260418001%22%2C%22zm_service_id%22%3A%222022111500000000000093666900%22%7D&charset=utf-8&format=JSON&method=zhima.credit.payafteruse.creditagreement.sign&sign=xxx&sign_type=RSA2&timestamp=2026-04-18+10%3A30%3A00&version=1.0",
  "schemeUrl": "alipays://platformapi/startapp?appId=20000067&url=https%3A%2F%2Frender.alipay.com%2Fp%2Fyuyan%2F180020010000706007%2Findex.html%3FcaprMode%3Dsync%26signStr%3Dxxx"
}
```

### 字段说明

| 字段 | 说明 |
|------|------|
| `signStr` | 签名字符串，用于客户端唤起支付宝 |
| `schemeUrl` | 完整的 scheme 链接，可直接在浏览器打开唤起支付宝 |

---

## 使用说明

1. 服务端调用此接口生成 `signStr` 和 `schemeUrl`
2. 客户端使用 `schemeUrl` 唤起支付宝 APP
3. 用户在支付宝中确认开通
4. 开通成功后，支付宝会回调通知商户
5. 商户可调用查询接口确认开通状态