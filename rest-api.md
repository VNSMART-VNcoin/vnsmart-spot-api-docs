- [General API Information](#general-api-information)
- [Error Codes](#error-codes)
- [Public API Endpoints](#public-api-endpoints)
	- [Reference Data endpoints](#reference-data-endpoints)
		- [Supported Currencies](#supported-currencies) 
	- [Market Data endpoints](#market-data-endpoints)
		- [Order book](#order-book)
		- [Trades](#trades)
		- [Ticker](#ticker)
		- [SummaryTicker](#summaryTicker)

# Public Rest API for Vnsmart

## General API Information
* The base endpoint is: **https://openapi.vnsmart.com**
* All endpoints return either a JSON object or array.

## HTTP Return Codes

* HTTP `5XX` return codes are used for internal errors; 


## Error Codes
* Any endpoint can return an ERROR

Sample Payload below:
```javascript
{
	"code": 1100002,
	"msg": "Spot does not exist.",
	"data": null
}
```

# Public API Endpoints

## Reference Data endpoints
### Supported Currencies
* This endpoint returns all Vnsmart supported tradable currencies.

```
GET /v1/common/currencys
```

**Parameters:**

--

**Response Example:**
```javascript
{
	"BTC": {
		"name": "Bitcoin",
		"unified_cryptoasset_id": "1",
		"can_withdraw": "false",
		"can_deposit": "false",
		"min_withdraw": "0.00100000",
		"max_withdraw": "10000.00000000",
		"maker_fee": "0.00200000",
		"taker_fee": "0.00200000"
	}
}
```
**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
name | String | YES | Full name of cryptocurrency.
unified_cryptoasset_id | Integer | NO | Unique ID of cryptocurrency assigned by Unified Cryptoasset ID.
can_withdraw | Boolean | YES | Identifies whether withdrawals are enabled or disabled.
can_deposit | Boolean | YES | Identifies whether deposits are enabled or disabled.
min_withdraw | Decimal | YES | Identifies the single minimum withdrawal amount of a cryptocurrency.
max_withdraw | Decimal | YES | Identifies the single maximum withdrawal amount of a cryptocurrency.
maker_fee | Decimal | YES | Fees applied when liquidity is added to the order book.
taker_fee | Decimal | YES | Fees applied when liquidity is removed from the order book.

## Market Data endpoints
### Order book
* This endpoint retrieves the current order book of a specific pair.

```
GET /v1/market/orderbook/marketPair
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
market_pair | String | YES |  A pair such as  “BTC_USDT”

**Response Example:**
```javascript
{
  "timestamp": "1643089953468",
  "bids": [
    [
      "36011.38850100",     // PRICE
      "0.18033350"    // QTY
    ]
  ],
  "asks": [
    [
      "36018.60150000",
      "0.00009352"
    ]
  ]
}
```
**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
timestamp | Integer | YES | Unix timestamp in milliseconds for when the last updated time occurred.
bids | Array | YES |The current all bids in format [price, size]
asks | Array | YES |The current all asks in format [price, size]

### Trades
* The trades endpoint is to return data on all recently completed trades for a given market pair.

```
GET /v1/market/trades/marketPair
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
market_pair | String | YES |  A pair such as  “BTC_USDT”

**Response Example:**
```javascript
[{
	"timestamp": "1643091817756",
	"trade_id": 2022012514233775601,
	"price": "36010.01",
	"base_volume": "0.50229100",
	"quote_volume": "18087.5039329100",
	"type": "sell"
}]
```
**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
timestamp | Integer | YES | Unix timestamp in milliseconds for when the transaction occurred.
trade_id | Integer | YES | A unique ID associated with the trade for the currency pair transaction
price | Decimal | YES |Last transacted price of base currency based on given quote currency
base_volume | Decimal | YES |Transaction amount in BASE currency.
quote_volume | Decimal | YES |Transaction amount in QUOTE currency.
type | String | YES |Used to determine whether or not the transaction originated as a buy or sell.  Buy – Identifies an ask was removed from the order book.Sell – Identifies a bid was removed from the order book.

### Ticker
* The ticker endpoint is to provide a 24-hour pricing and volume summary for each market pair available on the exchange.

```
GET /v1/common/ticker
```

**Parameters:**

--

**Response Example:**
```javascript
{
	"BTC_USDT": {
		"base_id": "1",
		"quote_id": "825",
		"last_price": "35856.20",
		"base_volume": "7198.382462",
		"quote_volume": "261533605.16",
		"isFrozen": "0"
	}
}
```
**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
base_id | Integer | NO | The base pair Unified Cryptoasset ID. 
quote_id | Integer | NO | The quote pair Unified Cryptoasset ID. 
last_price | Decimal | YES | Last transacted price of base currency based on given quote currency.
base_volume | Decimal | YES | 24-hour trading volume denoted in BASE currency.
quote_volume | Decimal | YES | 24 hour trading volume denoted in QUOTE currency.
isFrozen | Integer | YES | Indicates if the market is currently enabled (0) or disabled (1).

### SummaryTicker
* The summary endpoint is to provide an overview of market data for all tickers and all market pairs on the exchange.

```
GET /v1/common/tickers
```

**Parameters:**

--

**Response Example:**
```javascript
[{
	"trading_pairs": "BTC_USDT",
	"base_currency": "BTC",
	"quote_currency": "USDT",
	"last_price": "36026.79",
	"lowest_ask": "36046.73431300",
	"highest_bid": "36023.17732200",
	"base_volume": "7458.362601",
	"quote_volume": "270844131.24",
	"price_change_percent_24h": "0.07",
	"highest_price_24h": "37474.37",
	"lowest_price_24h": "33370.72"
},
{
	"trading_pairs": "ETH_USDT",
	"base_currency": "ETH",
	"quote_currency": "USDT",
	"last_price": "2377.75",
	"lowest_ask": "2382.35998000",
	"highest_bid": "2375.48214000",
	"base_volume": "57022.5655",
	"quote_volume": "136783282.55",
	"price_change_percent_24h": "0.06",
	"highest_price_24h": "2479.37",
	"lowest_price_24h": "2215.62"
}]
```
**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
trading_pairs | String | YES | Identifier of a ticker with delimiter to separate base/quote, eg. BTC_USDT (Price of BTC is quoted in USDT).
base_currency | String | YES | Symbol/currency code of base currency, eg. BTC.
quote_currency | String | YES | Symbol/currency code of quote currency, eg. USDT.
last_price | Decimal | YES | Last transacted price of base currency based on given quote currency.
lowest_ask | Decimal | YES | Lowest Ask price of base currency based on given quote currency.
highest_bid | Decimal | YES | Highest bid price of base currency based on given quote currency.
base_volume | Decimal | YES | 24-hr volume of market pair denoted in BASE currency.
quote_volume | Decimal | YES | 24-hr volume of market pair denoted in QUOTE currency.
price_change_percent_24h | Decimal | YES | 24-hr % price change of market pair.
highest_price_24h | Decimal | YES | Highest price of base currency based on given quote currency in the last 24-hrs.
lowest_price_24h | Decimal | YES | Lowest price of base currency based on given quote currency in the last 24-hrs
