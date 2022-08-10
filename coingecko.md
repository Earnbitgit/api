## Introduction

This document covers API endpoint details needed for integration of cryptocurrency exchanges, lending/borrowing platforms, as well as yield farms on CoinGecko.
For more information, please refer to:
- **Section for Spot Exchanges API**,

## Spot Exchanges
Spot Exchanges - Endpoints Overview
For CoinGecko Integration:

| No. | Endpoint | Description |
| ------ | ------ | ------ |
| 1. | /pairs | Details on cryptoassets traded on an exchange. |
| 2. | /tickers | Market related statistics for all markets for the last 24 hours. |
| 3. | /orderbook | Order book depth of any given trading pair, split into two different arrays for bid and ask orders. |
| 4. | /historical | Historical trade data for any given trading pair. |


### Endpoint 1 - Overview
<details  open>
<summary>
The /pairs endpoint provides a summary on cryptoasset trading pairs available on the exchange.
</summary>

```java
GET /api/gck/v1/pairs
```

```java
curl -X GET "https://api.earnbit.com/api/gck/v1/pairs" -H "accept: application/json"
```

For example, for BTC_USDT:
```java
  [
    {
        "ticker_id": "BTC_USDT",
        "base": "BTC",
        "target": "USDT"
    },
    ...
  ]
```
Endpoint response description:

| Name | Data Type | Category | Description |
| ------ | ------ | ------ | ------ |
| ticker_id | string | Mandatory | Identifier of a ticker with delimiter to separate base/target, eg. BTC_ETH |
| base | string | Mandatory | Symbol/currency code of a the base cryptoasset, eg. BTC |
| target | string | Mandatory | Symbol/currency code of the target cryptoasset, eg. ETH |

</details>


### Endpoint 2 - Market Info
<details  open>
<summary>
The /tickers endpoint provides 24-hour pricing and volume information on each market pair available on an exchange.
</summary>

```java
GET /api/gck/v1/tickers
```

```java
curl -X GET "https://api.earnbit.com/api/gck/v1/tickers" -H "accept: application/json"
```
Response:
```java
[
{
      "ticker_id": "BTC_ETH",
      "base_currency": "BTC",
      "target_currency": "ETH",
      "last_price":"50.0",
      "base_volume":"10",
      "target_volume":"500",
      "bid":"49.9",
      "ask":"50.1",
      "high":"51.3",
      "low":"49.2",
},
...
]
```
Endpoint response description:

| Name | Data Type | Category | Description |
| ------ | ------ | ------ | ------ |
|ticker_id|string|Mandatory|Identifier of a ticker with delimiter to separate base/target, eg. BTC_ETH|
|base_currency|string|Mandatory|Symbol/currency code of base pair, eg. BTC|
|target_currency|string|Mandatory|Symbol/currency code of target pair, eg. ETH|
|last_price|decimal|Mandatory|Last transacted price of base currency based on given target currency|
|base_volume|decimal|Mandatory|24 hour trading volume in base pair volume|
|target_volume|decimal|Mandatory|24 hour trading volume in target pair volume|
|bid|decimal|Mandatory|Current highest bid price|
|ask|decimal|Mandatory|Current lowest ask price|
|high|decimal|Mandatory|Rolling 24-hours highest transaction price|
|low|decimal|Mandatory|Rolling 24-hours lowest transaction price|
</details>


### Endpoint 3 - Order book depth details
<details  open>
<summary>
The /orderbook/ticker_id endpoint is to provide order book information with at least depth = 100 (50 each side) returned for a given market pair/ticker.
</summary>

Endpoint parameters:
|Name|Type|Status|Description|
| ------ | ------ | ------ | ------ |
|ticker_id|string|Mandatory|A ticker such as "BTC_ETH", with delimiter between different cryptoassets|
|depth|integer|Recommended|Orders depth quantity: [0, 100, 200, 500...]. 0 returns full depth. Depth = 100 means 50 for each bid/ask side. _Note that for more liquid or closely priced pairs, the lack of order depth may result in miscalculation of depth/spread_.|

```java
GET /api/gck/v1/orderbook/{ticker_id}
```
```java
curl -X GET 'https://api.earnbit.com/api/gck/v1/orderbook/BTC_USDT?depth=10'-H "accept: application/json"
```
Response:
```java
{
   "ticker_id": "BTC_ETH",
   "timestamp":"1700050000",
   "bids":[
      [
         "49.8",
         "0.50000000"
      ],
      [
         "49.9",
         "6.40000000"
      ]
   ],
   "asks":[
      [
         "50.1",
         "9.20000000"
      ],
      [
         "50.2",
         "7.9000000"
      ]
   ]
}
```
Order book response descriptions:
| Name | Data Type | Category | Description |
| ------ | ------ | ------ | ------ |
|ticker_id|string|Mandatory|A pair such as "BTC_ETH", with delimiter between different cryptoassets|
|timestamp|timestamp|Recommended|Unix timestamp in milliseconds for when the last updated time occurred.|
|bids|decimal|Mandatory|An array containing 2 elements. The offer price and quantity for each bid order|
|asks|decimal|Mandatory|An array containing 2 elements. The ask price and quantity for each ask order|
</details>


### Endpoint 4 - Historical Data
<details  open>
<summary>
The /historical_trades/ticker_id is used to return data on historical completed trades for a given market pair.
</summary>

Endpoint parameters:
|Name| Data Type |Category|Description|
| ------ | ------ | ------ | ------ |
|ticker_id|string|Mandatory|A pair such as "BTC_ETH", with delimiter between different cryptoassets|
|type|string|Mandatory|To indicate nature of trade - buy/sell|
|limit|integer|Recommended|Number of historical trades to retrieve from time of query. [0, 200, 500...]. 0 returns full history.|
|start_time|date|Recommended|Start time from which to query historical trades from|
|end_time|date|Recommended|End time for historical trades query|

```java
GET /api/gck/v1/historical_trades/{{ticker_id}}
```
```java
curl -X GET 'https://api.earnbit.com/api/gck/v1/historical_trades/BTC_USDT?limit=0' -H "accept: application/json"
```

Response:
```java
"buy": [
   {
      "trade_id":1234567,
      "price":"50.1",
      "base_volume":"0.1",
      "target_volume":"1",
      "trade_timestamp":"1700050000",
      "type":"buy"
   }
],
"Sell": [
   {
      "trade_id":1234567,
      "price":"50.1",
      "base_volume":"0.1",
      "target_volume":"1",
      "trade_timestamp":"1700050000",
      "type":"sell"
   }
]
```
Response descriptions:

|Name|Data Type|Category|Description|
| ------ | ------ | ------ | ------ |
|trade_id|integer|Mandatory|A unique ID associated with the trade for the currency pair transaction|
||||Note: Unix timestamp does not qualify as trade_id.|
|price|  decimal|Mandatory|Transaction price in base pair volume.|
|base_volume|  decimal|Mandatory|Transaction amount in base pair volume.|
|target_volume|  decimal|Mandatory|Transaction amount in target pair volume.|
|trade_timestamp|  timestamp|Mandatory|Unix timestamp in milliseconds for when the transaction occurred.|
|type|  string|Mandatory|Used to determine the type of the transaction that was completed.
||||Buy – Identifies an ask that was removed from the order book.|
||||Sell – Identifies a bid that was removed from the order book.|
</details>
