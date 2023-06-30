**For exchanges looking to integrate with CoinMarketCap, here are some guidelines.**

### ENDPOINT OVERVIEW

| Name | Category | Status | Description |
| ------ | ------ | ------ | ------ |
| Summary Endpoint | **[Summary](#summary-endpoint)** | Mandatory | Overview of market data for all tickers and all markets |
| Endpoint A1 | **[Assets](#assets-endpoint)** | Recommended | In depth details on crypto currencies available on the exchange. |
| Endpoint A2| **[Ticker](#ticker-endpoint)** | Mandatory | 24-hour rolling window price change statistics for all markets. |
| Endpoint A3| **[Orderbook](#orderbook-endpoint)** | Mandatory | Market depth of a trading pair. One array containing a list of ask prices and another array containing bid prices. Query for level 2 order book with full depth available as minimum requirement. |
| Endpoint A4| **[Trades](#trades-endpoint)** | Mandatory | Recently completed trades for a given market. 24 hour historical full trades available as minimum requirement. |

#### SUMMARY ENDPOINT

<details open>
  <summary>
  </summary>

```java
GET /api/cmc/v1/summary
```
```java
curl -X GET 'https://api.earnbit.com/api/cmc/v1/summary' -H "accept: application/json"
```

**Response Parameters:**

| Name | Type | Status |
| ------ | ------ | ------ |
| trading_pairs | string | Mandatory |
| base_currency | string | Recommended |
| quote_currency | string | Recommended |
| last_price | decimal | Mandatory |
| lowest_ask | decimal | Mandatory |
| highest_bid | decimal | Mandatory |
| base_volume | decimal | Mandatory |
| quote_volume | decimal | Mandatory |
| price_change_percent_24h | decimal | Mandatory |
| highest_price_24h | decimal | Mandatory |
| lowest_price_24h | decimal | Mandatory |

**Response**
```javascript
[
   {
        "trading_pairs": "YFI_USDT",
        "base_currency": "YFI",
        "quote_currency": "USDT",
        "last_price": 25477.88119744,
        "lowest_ask": 25492.06,
        "highest_bid": 25455.98,
        "base_volume": 261.8174298,
        "quote_volume": 6637837.775140016199,
        "price_change_percent_24h": -2,
        "highest_price_24h": 26239.192859,
        "lowest_price_24h": 24470.86
    },
    {
      ...
    }
]
```

</details>


#### ASSETS ENDPOINT
<details open>
  <summary>
  </summary>

```java
GET /api/cmc/v1/assets
```
```java
curl -X GET 'https://api.earnbit.com/api/cmc/v1/assets' -H "accept: application/json"
```

**Response Parameters:**

| Name | Type | Status |
| ------ | ------ | ------ |
| name | string | Recommended |
| unified_cryptoasset_id | integer | Recommended |
| can_withdraw | boolean | Recommended |
| can_deposit | boolean | Recommended |
| min_withdraw | decimal | Recommended |
| max_withdraw | decimal | Recommended |
| maker_fee | decimal | Recommended |
| taker_fee | decimal | Recommended |

**Response**

```javascript
{
    "KNC": {
        "name": "Kyber Network",
        "can_withdraw": true,
        "can_deposit": true,
        "min_withdraw": 0.000000000000000000,
        "max_withdraw": 0.000000000000000000,
        "maker_fee": 0.002,
        "taker_fee": 0.002
    },
    {
     ...
    }
}
```

</details>

#### TICKER ENDPOINT
<details open>
  <summary>
  </summary>

```java
GET /api/cmc/v1/ticker
```
```java
curl -X GET 'https://api.earnbit.com/api/cmc/v1/ticker' -H "accept: application/json"
```

**Response Parameters:**

| Name | Type | Status |
| ------ | ------ | ------ |
| base_id | integer | Recommended |
| quote_id | integer | Recommended |
| last_price | decimal | Mandatory |
| base_volume | decimal | Mandatory |
| quote_volume | decimal | Mandatory |
| is_frozen | decimal | Recommended |


**Response:**
```javascript
 {
    "BTC_USDT": {
        "last_price": "1",
        "quote_volume": "0",
        "base_volume": "0",
        "is_frozen": 0
    },
    {
      ...
    }
}
```

</details>

#### ORDERBOOK ENDPOINT
<details open>
  <summary>
  </summary>

```java
GET /api/cmc/v1/orderbook/{market_pair}
```
```java
curl -X GET "https://api.earnbit.com/api/cmc/v1/orderbook/BTC_USDT" -H "accept: application/json"
```

**Request Parameters**
| Name | Type | Status | Description
| ------ | ------ | ------ | ------ |
| **market_pair** | string | Mandatory | A pair such as “LTC_BTC” |
| depth | integer | Recommended (used to calculate liquidity score for rankings) | Orders depth quantity: [0,5,10,20,50,100,500] Not defined or 0 = full order book. Depth = 100 means 50 for each bid/ask side. |
| level | integer | Recommended(used to calculate liquidity score for rankings) | Level 1 – Only the best bid and ask. Level 2 – Arranged by best bids and asks. Level 3 – Complete order book, no aggregation. |

**Response Parameters:**
| Name | Type | Status | Description |
| ------ | ------ | ------ |  ------ |
| timestamp | integer | Mandatory | Unix timestamp in milliseconds for when the last updated time occurred |
| bids | decimal | Mandatory | An array containing 2 elements. The offer price and quantity for each bid order |
| asks | decimal | Mandatory | An array containing 2 elements. The ask price and quantity for each ask order. |

**Response:**
```javascript
 {
   "timestamp":"1585177482652",
   "bids":[
      [
         "12462000",
         "0.04548320"
      ],
      [
         "12457000",
         "3.00000000"
      ]
   ],
   "asks":[
      [
         "12506000",
         "2.73042000"
      ],
      [
         "12508000",
         "0.33660000"
      ]
   ]
}
```

</details>

#### TRADES ENDPOINT
<details open>
  <summary>
  </summary>

```java
GET /api/cmc/v1/trades/{market_pair}
```
```java
curl -X GET "https://api.earnbit.com/api/cmc/v1/trades/BTC_USDT" -H "accept: application/json"
```

**Request Parameters**
| Name | Type | Status | Description
| ------ | ------ | ------ | ------ |
| market_pair | string | Mandatory | A pair such as “LTC_BTC” |


**Response Parameters:**
| Name | Type | Status |
| ------ | ------ | ------ |
| trade_id | integer | Mandatory |
| price | decimal | Mandatory |
| base_volume | decimal | Mandatory |
| quote_volume | decimal | Mandatory |
| timestamp | integer (UTC timestamp in ms) | Mandatory |
| type | string | Mandatory |

**Response:**
```javascript
 [
    {
        "type": "sell",
        "price": 43504.86451672,
        "base_volume": 0.002487,
        "quote_volume": 108.19659805,
        "trade_id": 171663170,
        "timestamp": 1644584940
    },
    {
     ...
    }
]
```
</details>
