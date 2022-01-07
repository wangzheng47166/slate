# Orders

## 创建新订单

<font class="httppost">POST</font> */orders*


<aside>
REQUEST PARAMETERS
</aside>

**common parameters**

`product_id`: 必须是一个存在的产品id。例如 `BTC_USD`。产品列表可以通过 [products](#Products) 获得。

`side`: `buy` 买入， `sell` 卖出。

`type`: 订单类型

- `limit`: 限价订单
- `market`: 市价订单

`client_oid`: 可选。用户指定的订单号，用于用户管理自己的订单。该ID必须保证唯一，长度不多于64个字符的字符串类型

`stp`: 即 `self trade prevention`。BGE禁止用户与自己成交的行为。用户可以通过`stp`选项，指定当自成交场景出现时的订单处理策略。

- `dc`: 即 `decrease and cancel` (default)。当撮合发生相同用户订单匹配时，数量多的一方将被执行`decrease`指令，数量少的一方将被执行`cancel`指令。decrease 或者
  或者cancel 的数量为两个订单发生自成交匹配的最小值。
- `co`: 即 `cancle oldest`, 当撮合发生相同用户订单匹配时，挂单较早的订单会被执行`cancle`指令。新订单将被继续执行正常交易撮合流程。
- `cn`: 即 `cancle newest`, 当撮合发生相同用户订单匹配时，最新订单会被执行`cancle`指令，较早的挂单依然停留在订单簿，执行正常的交易撮合流程。
- `cb`: 即 `cancle both`,当撮合发生相同用户订单匹配时，发生匹配的两个订单，均会被执行`cancle`指令。

`time_in_force`: 可选，交易指令，目前支持 `GTC`。默认值为 `GTC`

**limit order parameters**

`price`: 商品价格
`size`: 买入或者卖出商品的数量

**market order parameters**
`size`: 期望交易数量。需要`side` 为 `sell`，代表以最新成交价进行卖出，期望最多卖出的商品数。
`funds`: 期望交易额度。需要`side` 为 `buy`，代表以最新成交价进行买入，期望花费的最多资产额度。

| 参数名称 | 参数说明 | 是否必须 | 数据类型 | schema |
| -------- | -------- | -------- | -------- | ------ |
|product_id|币对，例:BTC_USDT||true|string||
|side|buy/sell||true|string||
|type|limit:限价单/market:市价单||true|string||
|client_oid|用户侧关联订单||false|string||
|stp|自成交：dc：减少和取消（默认）co：取消最旧 cn：取消最新 cb：取消两者||false|string||
|time_in_force|交易指令,GTC||false|string||
|funds|想要使用的报价货币数量||false|string||
|price|每个币的价格||false|string||
|size|买入或卖出的数量||false|string||

<aside>
RESPONSE STATUS
</aside>

Status Code | Meaning | Example
---------- | ------- | --------
200 | Success Request | [参考示例](#ResonpseExample1)
401 | Unauthorized -- Your API key is wrong, See [鉴权说明](#auth) | <code>message</code> string
500 | Internal Server Error -- We had a problem with our server. Try again later. | <code>message</code> string

<aside>
RESPONSE PARAMETERS
</aside>

`id`:  BGE所生成的订单号

| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|id|BGE所生成的订单号|string||

> <a name="ResonpseExample">RESONPSE EXAMPLE</a>

```json
{
  "id": "128527387912385665"
}
```

## 查询单个订单

<a name="order_detail"></a>

<font class="httpget">GET</font> */orders/{order_id}*

<aside>
PATH VARIABLES
</aside>

| 参数名称 | 参数说明     | 是否必须 | 数据类型 | schema |
| -------- | ----- | -------- | -------- | ------ |
|order_id|订单ID|true|string||

<aside>
RESPONSE STATUS
</aside>

Status Code | Meaning | Example
---------- | ------- | --------
200 | Success Request | [参考示例](#order_detail_demo)
401 | Unauthorized -- Your API key is wrong, See [鉴权说明](#auth) | <code>message</code> string
500 | Internal Server Error -- We had a problem with our server. Try again later. | <code>message</code> string

<aside>
RESPONSE PARAMETERS
</aside>

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量

| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|filled_fees|成交费用|string||
|filled_size|成交金额|string||
|funds|想要使用的报价货币数量|string||
|order_id|订单编号|string||
|price|每单位基础货币的价格|string||
|product_id|产品编号|string||
|side|buy/sell|string||
|size|买入/卖出的基础货币数量|string||
|status|状态|string||
|type|limit:限价单/market:市价单/stop|string||

<aside>
RESPONSE EXAMPLE
</aside>

<a name="order_detail_demo"></a>

```json
{
  "filled_fees": "0.0012",
  "filled_size": "0.12",
  "funds": "0.3",
  "order_id": "128527387912385665",
  "price": "40000.0001",
  "product_id": "BTC_USD",
  "side": "buy",
  "size": "10",
  "status": "1",
  "type": "limit"
}
```

## 获取交易明细

<font class="httpget">GET</font> */fills*


<aside>
REQUEST PARAMETERS
</aside>

此接口可查询单个订单的交易明细，以及币种的多个明细。

- `order_id` 若存在，将优先查找该订单的成交明细，其他参数将被忽略。
- `order_id` 若不存在，将根据参数组合分页查询所所有交易明细。其中 `limit` 最大值为1000，超过1000条的订单明细将无法查询

| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|after|用于分页。将结束光标设置为after日期。|query|false|integer(int64)||
|before|用于分页。将开始光标设置为before日期|query|false|integer(int64)||
|limit|限制返回的结果数|query|false|integer(int32)||
|product_id|商品id|query|false|string||
|order_id|订单id|query|false|string||

<aside>
RESPONSE STATUS
</aside>

Status Code | Meaning | Example
---------- | ------- | --------
200 | Success Request | [参考示例](#order_detail_demo)
401 | Unauthorized -- Your API key is wrong, See [鉴权说明](#auth) | <code>message</code> string
500 | Internal Server Error -- We had a problem with our server. Try again later. | <code>message</code> string

<aside>
RESPONSE PARAMETERS
</aside>

| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|created_at|订单创建时间|string||
|fee|按当前填写的金额支付的费用|string||
|liquidity|流动性:marker、taker|string||
|order_id|订单编号|string||
|price|每单位基础货币的价格|string||
|product_id|产品编号|string||
|side| buy/sell |string||
|size|买入/卖出的基础货币数量|string||

<aside>
RESPONSE EXAMPLE
</aside>

<a name="fills_detail_demo"></a>

```json
[
  {
    "created_at": "",
    "fee": "",
    "liquidity": "",
    "order_id": "",
    "price": "",
    "product_id": "",
    "side": "",
    "size": "",
    "user_id": ""
  }
]
```

## 获取当前的未完结订单

<font class="httpget">GET</font> */orders*


<aside>
REQUEST PARAMETERS
</aside>

此接口可查询单个订单详情，或根据商品的多个订单

- `order_id` 若存在，将优先查找该订单详情，其他参数将被忽略。
- `order_id` 若不存在，将根据参数组合分页查询所所有订单详情。其中 `limit` 最大值为1000，超过1000条的订单详情将无法查询

| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|after|用于分页。将结束光标设置为after日期。|query|false|integer(int64)||
|before|用于分页。将开始光标设置为before日期|query|false|integer(int64)||
|limit|限制返回的结果数|query|false|integer(int32)||
|order_id|订单id|query|false|string||
|product_id|商品id|query|false|string||

<aside>
RESPONSE STATUS
</aside>

Status Code | Meaning | Example
---------- | ------- | --------
200 | Success Request | [参考示例](#order_trade_detail_demo)
401 | Unauthorized -- Your API key is wrong, See [鉴权说明](#auth) | <code>message</code> string
500 | Internal Server Error -- We had a problem with our server. Try again later. | <code>message</code> string

<aside>
RESPONSE PARAMETERS
</aside>

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量

| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|filled_fees|成交费用|string||
|filled_size|成交金额|string||
|funds|想要使用的报价货币数量|string||
|order_id|订单编号|string||
|price|每单位基础货币的价格|string||
|product_id|产品编号|string||
|side|buy/sell|string||
|size|买入/卖出的基础货币数量|string||
|status|状态|string||
|type|limit:限价单/market:市价单/stop|string||

<aside>
RESPONSE EXAMPLE
</aside>

<a name="order_trade_detail_demo"></a>

```json
{
  "filled_fees": "0.0012",
  "filled_size": "0.12",
  "funds": "0.3",
  "order_id": "128527387912385665",
  "price": "40000.0001",
  "product_id": "BTC_USD",
  "side": "buy",
  "size": "10",
  "status": "1",
  "type": "limit"
}
```

## 撤销单个订单

<font class="httpdelete">DELETE</font> */orders/{order_id}*


<aside>
REQUEST PARAMETERS
</aside>

此接口可查询单个订单详情，或根据商品的多个订单

- `order_id` 若存在，将优先查找该订单详情，其他参数将被忽略。
- `order_id` 若不存在，将根据参数组合分页查询所所有订单详情。其中 `limit` 最大值为1000，超过1000条的订单详情将无法查询

| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|after|用于分页。将结束光标设置为after日期。|query|false|integer(int64)||
|before|用于分页。将开始光标设置为before日期|query|false|integer(int64)||
|limit|限制返回的结果数|query|false|integer(int32)||
|order_id|订单id|query|false|string||
|product_id|商品id|query|false|string||

<aside>
RESPONSE STATUS
</aside>

Status Code | Meaning | Example
---------- | ------- | --------
200 | Success Request | [参考示例](#order_trade_detail_demo)
401 | Unauthorized -- Your API key is wrong, See [鉴权说明](#auth) | <code>message</code> string
500 | Internal Server Error -- We had a problem with our server. Try again later. | <code>message</code> string

<aside>
RESPONSE PARAMETERS
</aside>

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量

| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|filled_fees|成交费用|string||
|filled_size|成交金额|string||
|funds|想要使用的报价货币数量|string||
|order_id|订单编号|string||
|price|每单位基础货币的价格|string||
|product_id|产品编号|string||
|side|buy/sell|string||
|size|买入/卖出的基础货币数量|string||
|status|状态|string||
|type|limit:限价单/market:市价单/stop|string||

<aside>
RESPONSE EXAMPLE
</aside>

<a name="order_trade_detail_demo"></a>

```json
{
  "filled_fees": "0.0012",
  "filled_size": "0.12",
  "funds": "0.3",
  "order_id": "128527387912385665",
  "price": "40000.0001",
  "product_id": "BTC_USD",
  "side": "buy",
  "size": "10",
  "status": "1",
  "type": "limit"
}
```

## 取消所有订单

<font class="httpdelete">DELETE</font> */orders*


此方法为异步方法，当用户收到接口返回时，并不代表所有订单已经取消成功。BGE收到请求之后，会查询用户账户下对应商品ID的所有未成交订单，并对这些订单异步进行取消操作。
用户可以通过[/orders/{order_id}](#order_detail) 查询单个订单的成交状态。

<aside>
REQUEST PARAMETERS
</aside>

`product_id`: 被撤销的商品ID,例如 "BTC_USD"

| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | 
| -------- | -------- | ----- | -------- | -------- |
|product_id|商品id|query|true|string||

<aside>
RESPONSE STATUS
</aside>

Status Code | Meaning | Example
---------- | ------- | --------
200 | Success Request | [参考示例](#order_trade_detail_demo)
401 | Unauthorized -- Your API key is wrong, See [鉴权说明](#auth) | <code>message</code> string
500 | Internal Server Error -- We had a problem with our server. Try again later. | <code>message</code> string

<aside>
RESPONSE PARAMETERS
</aside>

即将被取消的订单的ID列表

```json
[
  "128527387912385665"
]
```
