# API 参考文档

本文档提供接口索引、命令格式及使用示例。详细的请求/响应报文请查看各接口的示例报文链接。

## 产品信息

- **产品名称**: 芝麻先享（动态金额模式）
- **产品描述**: 动态金额模式下先享服务下单金额是不确定的。服务到期时，商家基于实际金额发起扣款，例如物流快递场景。
- **官方文档**: https://opendocs.alipay.com/open/03vtf0
- **接入说明**: 芝麻先享仅支持自研商家/服务商通过自研应用或第三方应用代调用方式接入

---

## 接口列表

### 开通服务相关接口

| 接口描述 | API接口 | 说明 | 官方文档 | 示例报文 |
|---------|---------|------|----------|----------|
| 信用服务开通/授权 | `zhima.credit.payafteruse.creditagreement.sign` | 建立商家与用户关系，获取开通协议号 | [查看文档](https://opendocs.alipay.com/open/4580646b_zhima.credit.payafteruse.creditagreement.sign) | [示例](examples/sign-request.md) |
| 查询服务开通/授权信息 | `zhima.credit.payafteruse.creditagreement.query` | 查询用户开通状态及相关信息 | [查看文档](https://opendocs.alipay.com/open/999ae950_zhima.credit.payafteruse.creditagreement.query) | [示例](examples/query-agreement-request.md) |
| 服务开通/授权状态变更通知 | `zhima.credit.payafteruse.creditagreement.changed` | 消息通知类接口 | [查看文档](https://opendocs.alipay.com/open/50885f82_zhima.credit.payafteruse.creditagreement.changed) | - |

### 信用下单相关接口

| 接口描述 | API接口 | 说明 | 官方文档 | 示例报文 |
|---------|---------|------|----------|----------|
| 信用服务下单（免用户确认场景） | `zhima.credit.payafteruse.creditbizorder.order` | 免密场景下单 | [查看文档](https://opendocs.alipay.com/open/144e086e_zhima.credit.payafteruse.creditbizorder.order) | [示例](examples/order-request.md) |
| 信用服务下单（用户确认场景） | `zhima.credit.payafteruse.creditbizorder.create` | 用户拉端确认下单 | [查看文档](https://opendocs.alipay.com/open/dfb97313_zhima.credit.payafteruse.creditbizorder.create) | [示例](examples/create-order-request.md) |
| 信用服务订单查询 | `zhima.credit.payafteruse.creditbizorder.query` | 查询订单状态及信息 | [查看文档](https://opendocs.alipay.com/open/4b0b26e9_zhima.credit.payafteruse.creditbizorder.query) | [示例](examples/query-request.md) |
| 信用服务订单状态变更通知 | `zhima.credit.payafteruse.creditbizorder.changed` | 订单状态变动通知 | [查看文档](https://opendocs.alipay.com/open/6c3a11df_zhima.credit.payafteruse.creditbizorder.changed) | - |

### 扣款相关接口

| 接口描述 | API接口 | 说明 | 官方文档 | 示例报文 |
|---------|---------|------|----------|----------|
| 扣款 | `alipay.trade.pay` | 信用订单扣款 | [查看文档](https://opendocs.alipay.com/open/507b8b31_alipay.trade.pay) | [示例](examples/pay-request.md) |
| 退款 | `alipay.trade.refund` | 扣款成功后退款 | [查看文档](https://opendocs.alipay.com/open/ce7e9000_alipay.trade.refund) | [示例](examples/refund-request.md) |
| 交易查询 | `alipay.trade.query` | 查询扣款结果 | [查看文档](https://opendocs.alipay.com/open/1bce7243_alipay.trade.query) | [示例](examples/query-trade-request.md) |

### 结束订单接口

| 接口描述 | API接口 | 说明 | 官方文档 | 示例报文 |
|---------|---------|------|----------|----------|
| 结束信用服务订单 | `zhima.credit.payafteruse.creditbizorder.finish` | 完结或取消信用服务订单 | [查看文档](https://opendocs.alipay.com/open/31ea8bd1_zhima.credit.payafteruse.creditbizorder.finish) | [示例](examples/finish-request.md) |

> 💡 **提示**：点击"示例报文"链接查看完整的请求/响应报文及字段说明。

---

## 命令格式

```bash
# 开通服务（生成签名字符串，由客户端唤起支付宝）
node scripts/zhima-credit-payafteruse.js sign --out_agreement_no "<商户外部协议号>" --zm_service_id "<服务ID>"

# 查询协议开通信息（二选一参数）
node scripts/zhima-credit-payafteruse.js query-agreement --credit_agreement_id "<协议ID>"
node scripts/zhima-credit-payafteruse.js query-agreement --out_agreement_no "<商户外部协议号>"

# 创建订单（免用户确认场景）
node scripts/zhima-credit-payafteruse.js order --credit_agreement_id "<协议ID>" --out_order_no "<订单ID>" --subject "<订单标题>"

# 创建订单（用户确认场景，生成签名字符串）
node scripts/zhima-credit-payafteruse.js create-order --zm_service_id "<服务ID>" --out_order_no "<订单ID>" --subject "<订单标题>"

# 查询订单
node scripts/zhima-credit-payafteruse.js query --order_id "<订单ID>"

# 发起扣款
node scripts/zhima-credit-payafteruse.js pay --order_id "<订单ID>" --total_amount "<金额>" --subject "<订单标题>"

# 查询交易
node scripts/zhima-credit-payafteruse.js query-trade --trade_no "<支付宝交易号>"

# 结束订单
node scripts/zhima-credit-payafteruse.js finish --credit_biz_order_id "<订单ID>" --is_fulfilled <true|false>

# 退款
node scripts/zhima-credit-payafteruse.js refund --trade_no "<支付宝交易号>" --refund_amount "<退款金额>"

# 查看本次API请求报文
node scripts/zhima-credit-payafteruse.js last-request

# 查看本次API响应报文
node scripts/zhima-credit-payafteruse.js last-result
```

---

## 使用示例

### 开通服务

```bash
node scripts/zhima-credit-payafteruse.js sign --out_agreement_no "2014070700166653" --zm_service_id "2022111500000000000093666900"
```

### 查询协议开通信息

**方式一：使用协议号**
```bash
node scripts/zhima-credit-payafteruse.js query-agreement --credit_agreement_id "ZM2021071900007000001"
```

**方式二：使用商户外部协议号**
```bash
node scripts/zhima-credit-payafteruse.js query-agreement --out_agreement_no "2014070700166653"
```

### 创建订单

**免确认场景**
```bash
node scripts/zhima-credit-payafteruse.js order --credit_agreement_id "ZM2021071900007000001" --out_order_no "ORDER20210719001" --subject "快递服务费"
```

**用户确认场景**
```bash
node scripts/zhima-credit-payafteruse.js create-order --zm_service_id "2022111500000000000093666900" --out_order_no "ORDER20210719002" --subject "快递服务费"
```

### 查询订单

```bash
node scripts/zhima-credit-payafteruse.js query --order_id "ZMCB99202107190000070000051245"
```

### 发起扣款

```bash
node scripts/zhima-credit-payafteruse.js pay --order_id "ZMCB99202107190000070000051245" --total_amount "88.88" --subject "快递服务费"
```

### 查询交易

```bash
node scripts/zhima-credit-payafteruse.js query-trade --trade_no "2021071900007000001"
```

### 结束订单

```bash
node scripts/zhima-credit-payafteruse.js finish --credit_biz_order_id "ZMCB99202107190000070000051245" --is_fulfilled true
```

### 退款

```bash
node scripts/zhima-credit-payafteruse.js refund --trade_no "2021071900007000001" --refund_amount "50.00"
```

---

## 附录

### 订单状态说明

| 状态 | 说明 |
|------|------|
| INIT | 下单状态 |
| TRADE_CLOSED | 订单取消或者交易全额退款 |
| TRADE_FINISHED | 扣款成功状态 |

### 订单号格式说明

| 类型 | 格式 | 示例 |
|------|------|------|
| 信用订单号 | 以 `ZMCB` 开头，后跟 20-30 位数字 | `ZMCB99202107190000070000051245` |
| 商户订单号 | 8-64 位字母、数字、下划线或横线组合 | `ORDER20210719001` |
| 支付宝交易号 | 20-32 位纯数字 | `2026041012345678901234567890` |

### sdkExecute 模式说明

`sign` 和 `create-order` 接口使用支付宝 SDK 的 `sdkExecute` 方式，不走常规 JSON 请求/响应。

服务端生成签名字符串，由客户端使用该字符串唤起支付宝完成用户确认：

- 脚本会同时输出 `signStr`（签名字符串）和 `schemeUrl`（完整 scheme 链接）
- `schemeUrl` 可直接在浏览器或客户端打开唤起支付宝完成确认