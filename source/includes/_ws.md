# WEBSOCKET FEED PUBLIC

- ` {host}/ws`

## Candles Subscribe/UnSubscribe 

#### Subscribe

> request

```json
	{
		"event": "sub",
		"params": {
			"biz": "market",
			"type": "candles",
			"pairCode": "ETH_BNB",
			"interval": "1day"
		},
		"zip":false
	}	
```

#### 请求参数

|参数名|必选|类型| 说明   |
|:----    |:---|:----- |------|
| params .biz | 是  |string |      |
| params .type     |是  |string | 订阅类型类型 |
| params .pairCode     |是  |string | 币对   |
| params .interval     |是  |string | 周期   |
| event     |是  |string | 事件 Subscribe  |
| zip     |是  |bool | 是否启用gzip |


> response

```json
{
	"id": "",
	"channel": "subscribe",
	"biz": "market",
	"type": "candles",
	"pairCode": "ETH_BNB",
	"interval": "1day",
	"ts": 1639655425910,
	"status": "ok"
}
```

#### 返回参数
|参数名|类型|说明|
|:----    |:----- |-----   |
| type | string |  订阅类型 |
| interval     |string | 间隔  |
| pairCode    |string | 币对|
| data .open     |string | 开盘 |
| data .close    |string | 收盘 |
| data .low     |string | 低 |
| data .high     |string | 高 |
| data .vol     |string | 成交 |
| data .turnOver     |string | 成交量 |
| data .count     |string |  |


#### UnSubscribe

> request

```json
	{
		"event": "unsub",
		"params": {
			"biz": "market",
			"type": "candles",
			"pairCode": "ETH_BNB",
			"interval": "1day"
		},
		"zip":true
	}	
```

#### 请求参数

|参数名|必选|类型| 说明            |
|:----    |:---|:----- |---------------|
| params .biz | 是  |string |               |
| params .type     |是  |string | 订阅类型类型        |
| params .pairCode     |是  |string | 币对            |
| params .interval     |是  |string | 周期            |
| event     |是  |string | 事件 UnSubscribe |
| zip     |是  |bool | 是否启用gzip      |


> response

```json
{
	"id": "",
	"channel":"unsubscribe",
	"biz": "market",
	"type": "candles",
	"pairCode": "ETH_BNB",
	"interval": "1day",
	"ts": 1639655425910,
	"status": "ok"
}
```

#### 返回参数
|参数名|类型|说明|
|:----    |:----- |-----   |
| type | string |  订阅类型 |
| interval     |string | 间隔  |
| pairCode    |string | 币对|
| data .open     |string | 开盘 |
| data .close    |string | 收盘 |
| data .low     |string | 低 |
| data .high     |string | 高 |
| data .vol     |string | 成交 |
| data .turnOver     |string | 成交量 |
| data .count     |string |  |



#### FEED 


```json

{
	"id": "his",
	"type": "candles",
	"pairCode": "ETH_USDT",
	"interval": "1day",
	"data": [{
		"id": 1639584000,
		"open": 11,
		"close": 11.9,
		"low": 11,
		"high": 11.9,
		"vol": 2,
		"turnOver": 22.9,
		"count": 2
	}, {
		"id": 1639670400,
		"open": 11.9,
		"close": 11.9,
		"low": 11.9,
		"high": 11.9,
		"vol": 0,
		"turnOver": 0,
		"count": 0
	}]
}

```
----

##  Fills Subscribe/UnSubscribe

#### Subscribe


```json
{
  "event": "sub",
  "params": {
    "biz": "market",
    "type": "fills",
    "pairCode": "ETH_USDT"
  },
  "zip": false
}
```


|参数名|必选|类型| 说明       |
|:----    |:---|:----- |----------|
| params .biz | 是  |string |          |
| params .type     |是  |string | 订阅类型类型   |
| params .pairCode     |是  |string | 币对       |
| params .interval     |是  |string | 周期       |
| event     |是  |string | 事件 Subscribe      |
| zip     |是  |bool | 是否启用gzip |



```json
{
  "id": "",
  "channel": "subscribe",
  "biz": "market",
  "type": "fills",
  "pairCode": "ETH_USDT",
  "interval": "",
  "ts": 1641384262679,
  "status": "ok"
}
```

#### UnSubscribe

|参数名|必选|类型| 说明       |
|:----    |:---|:----- |----------|
| params .biz | 是  |string |          |
| params .type     |是  |string | 订阅类型类型   |
| params .pairCode     |是  |string | 币对       |
| params .interval     |是  |string | 周期       |
| event     |是  |string | 事件 Subscribe      |
| zip     |是  |bool | 是否启用gzip |

```json

{
	"event": "unsub",
	"params": {
		"biz": "market",
		"type": "fills",
		"pairCode": "ETH_USDT"
	},
	"zip": false
}

```


```json
{
	"id": "",
	"unsubbed": "market.ETH_USDT.trade.detail",
	"ts": 1641384470335,
	"status": "ok"
}
```
#### FEED 

|参数名|类型|说明|
|:----    |:---|:----- |-----   |
| type | string |  订阅类型 |
| interval     |string | 间隔  |
| pairCode    |string | 币对|
| data .price     |string | 价格 |
| data .vol     |string | 成交 |
| data .ts     |int  | 时间戳 |
| data .direction     |string |  方向|

```json

{
	"biz": "market",
	"data": [{
		"vol": "1",
		"ts": 1639585820373,
		"id": 28470000,
		"price": "11",
		"direction": "buy"
	}, {
		"vol": "1",
		"ts": 1639585820376,
		"id": 28730000,
		"price": "11.9",
		"direction": "sell"
	}],
	"id": "his",
	"pairCode": "ETH_USDT",
	"ts": 1640919016188,
	"type": "fills"
}

``` 

-----

## OrderBook Subscribe/UnSubscribe

> request

```json
{
  "event": "sub",
  "params": {
    "biz": "market",
    "type": "orderBook",
    "pairCode": "ETH_USDT",
    "interval": "1"
  },
  "zip": false
}
```

#### Subscribe

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params .biz | 是  |string |   |
| params .type     |是  |string | 订阅类型类型  |
| params .pairCode     |是  |string | 币对|
| params .interval     |是  |string | _周期_ |
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |


> response

```json
{
  "id": "",
  "channel": "subscribe",
  "biz": "market",
  "type": "orderBook",
  "pairCode": "ETH_USDT",
  "interval": "step1",
  "ts": 1641384620237,
  "status": "ok"
}
```

#### UnSubscribe 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params .biz | 是  |string |   |
| params .type     |是  |string | 订阅类型类型  |
| params .pairCode     |是  |string | 币对|
| params .interval     |是  |string | _周期_ |
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |

```json
{
  "event": "unsub",
  "params": {
    "biz": "market",
    "type": "orderBook",
    "pairCode": "ETH_USDT",
    "interval": "1"
  },
  "zip": false
}

```

```json
{
  "id": "",
  "unsubbed": "market.ETH_USDT.depth.step1",
  "ts": 1641384766983,
  "status": "ok"
}

```

#### FEED 



```json

{
	"seqId": 8414921,
	"id": 1641385128,
	"bids": [
		[3781.85, 0.30656],
		[3781.78, 0.0219],
		[360, 4.59207],
		[10, 11]
	],
	"asks": [
		[3835, 0.19948],
		[3836.98, 0.00026],
		[3837.33, 0.4445],
		[3839, 0.2]
	],
	"ts": 1641385128493,
	"version": 1641385128,
	"type": "orderBook",
	"pairCode": "ETH_USDT",
	"interval": "1"
}
```
----

## Percent10 Subscribe\Unsubscribe

#### Subscribe

> request

```json
	{
		"event": "sub",
		"params": {
			"type": "percent10",
			"pairCode": "ETH_USDT"
		},
		"zip":false
	}	
```

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params .biz | 是  |string |   |
| params .type     |是  |string | 订阅类型类型  |
| params .pairCode     |是  |string | 币对|
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |
```json
{
  "id": "",
  "channel": "subscribe",
  "biz": "market",
  "type": "percent10",
  "pairCode": "ETH_USDT",
  "interval": "",
  "ts": 1641387412252,
  "status": "ok"
}
```
#### UnSubscribe

> request

```json
	{
		"event": "unsub",
		"params": {
			"type": "percent10",
			"pairCode": "ETH_USDT"
		},
		"zip":false
	}	
```

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params .biz | 是  |string |   |
| params .type     |是  |string | 订阅类型类型  |
| params .pairCode     |是  |string | 币对|
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |

```json
{
  "id": "",
  "unsubbed": "market.ETH_USDT.depth.percent10",
  "ts": 1641387599816,
  "status": "ok"
}
```


#### FEED

```json
{
	"seqId": 2875,
	"id": 1640683777,
	"bids": [
		[11.994, 0.10124]
	],
	"asks": [
		[12.006, 0]
	],
	"ts": 1640683777520,
	"version": 1640683777,
	"type": "percent10",
	"pairCode": "ETH_USDT"
}
```

----


##  Ticker Subscribe\UnSubscribe



#### Subscribe

> request

```json
	{
		"event": "sub",
		"params": {
			"biz": "market",
			"type": "ticker",
			"pairCode": "ETH_USDT"
		},
		"zip":false
	}	
```


|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params .biz | 是  |string |   |
| params .type     |是  |string | 订阅类型类型  |
| params .pairCode     |是  |string | 币对|
| params .interval     |是  |string | 周期 |
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |


> response

```json
{
  "id": "",
  "channel": "subscribe",
  "biz": "market",
  "type": "ticker",
  "pairCode": "ETH_USDT",
  "interval": "",
  "ts": 1641387679813,
  "status": "ok"
}
```

#### Subscribe

> request

```json
	{
		"event": "unsub",
		"params": {
			"biz": "market",
			"type": "ticker",
			"pairCode": "ETH_USDT"
		},
		"zip":false
	}	
```


|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params .biz | 是  |string |   |
| params .type     |是  |string | 订阅类型类型  |
| params .pairCode     |是  |string | 币对|
| params .interval     |是  |string | 周期 |
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |


> response

```json
{
  "id": "",
  "unsubbed": "market.ETH_USDT.ticker",
  "ts": 1641387799984,
  "status": "ok"
}
```
#### FEED 

|参数名|类型|说明|
|:----    |:----- |-----   |
| type | string |  订阅类型 |
| interval     |string | 间隔  |
| pairCode    |string | 币对|
| data .open     |string | 开盘 |
| data .close    |string | 收盘 |
| data .low     |string | 低 |
| data .high     |string | 高 |
| data .vol     |string | 成交 |
| data .turnOver     |string | 成交量 |
| data .count     |string |  |

> push 

```json

{
	"biz": "market",
	"data": {
		"open": "18",
		"close": "19.56",
		"low": "18",
		"high": "19.56",
		"turnOver": "68.46",
		"count": 0,
		"vol": "3.5",
		"pairCode": "BTC_USDT",
		"change": "1.56",
		"changePercent": "0.0866666666666667"
	},
	"id": "",
	"pairCode": "BTC_USDT",
	"ts": 1640251563497,
	"type": "ticker"
}
```
----


## Request Candles

> request 

```json
{
	"event": "req",
	"params": {
		"biz": "market",
		"type": "candles",
		"pairCode": "ETH_USDT",
		"interval": "1day",
		"size": 301,
		"from": 1613692800,
		"to": 1639699200,
		"id": "his"
	},
	"zip":false
}
```
| 参数名             | 必选  |类型| 说明       |
|:----------------|:----|:----- |----------|
| params.biz      | 是   |string |          |
| params.type     | 是   |string | 类型       |
| params.pairCode | 是   |string | 币对       |
| params.interval | 是   |string | 周期       |
| params.from     | 是   |string | 开始时间     |
| params.to       | 是   |string | 结束时间     |
| event           | 是   |string | 事件       |
| zip             | 是   |bool | 是否启用gzip |


###### 返回结构

|参数名|类型|   说明 |
|:------:|:----:|:--:|
| close | int  |   收盘价格 |
| high | int  |   最高价格 |
| low | int  |   最低价格 |
| open | int  |  开盘价格 |
| turnOver | string  |  交易量 |
| vol | string  |  交易额 |
| pairCode | string  |  币对  |


```json
{
	"id": "his",
	"type": "candles",
	"pairCode": "ETH_USDT",
	"interval": "1day",
	"data": [{
		"id": 1639584000,
		"open": 11,
		"close": 11.9,
		"low": 11,
		"high": 11.9,
		"vol": 2,
		"turnOver": 22.9,
		"count": 2
	}, {
		"id": 1639670400,
		"open": 11.9,
		"close": 11.9,
		"low": 11.9,
		"high": 11.9,
		"vol": 0,
		"turnOver": 0,
		"count": 0
	}]
}


```

## Request Ticker

#### 请求结构

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| params.biz | 是  |string |   |
| params.type     |否  |string | 类型 |
| params.pairCode     |否  |string | 币对|
| event     |是  |string | 事件 |
| zip     |是  |bool | 是否启用gzip |

```json
	{
	"event": "req",
	"params": {
		"biz": "market",
		"type": "ticker",
		"pairCode": "ETH_USDT",
		"id": "his"
	},
	"zip":false
}
```

#### 返回参数

|参数名|类型|   说明 |
|:------:|:----:|:--:|
| close | int  |   收盘价格 |
| high | int  |   最高价格 |
| low | int  |   最低价格 |
| open | int  |  开盘价格 |
| turnOver | string  |  交易量 |
| vol | string  |  交易额 |
| pairCode | string  |  币对  |

```json
{
	"biz": "market",
	"data": {
		"open": "18",
		"close": "19.56",
		"low": "18",
		"high": "19.56",
		"turnOver": "68.46",
		"count": 0,
		"vol": "3.5",
		"pairCode": "BTC_USDT",
		"change": "1.56",
		"changePercent": "0.0866666666666667"
	},
	"id": "",
	"pairCode": "BTC_USDT",
	"ts": 1640251563497,
	"type": "ticker"
}
```

