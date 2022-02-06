- [General API Information](#general-api-information)
- [Error Codes](#error-codes)
- [Public API Endpoints](#public-api-endpoints)
  - [Reference Data endpoints](#reference-data-endpoints)
    - [Supported Pairs](#supported-pairs)
  - [Market Data endpoints](#market-data-endpoints)
    - [Order book](#order-book)
    - [Trades](#trades)
    - [Ticker](#ticker)

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

### Supported Pairs

* Details on cryptoassets traded on an exchange.

```
GET /v1/coingecko/pairs
```

**Parameters:**

--

**Response Example:**

```javascript
[{
	"base": "BTC",
	"target": "USDT",
	"ticker_id": "BTC_USDT"
}]
```

**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
base| String | YES | Symbol/currency code of a the base cryptoasset, eg. BTC.
target| String| YES | Symbol/currency code of the target cryptoasset, eg. USDT.
ticker_id| String| YES | Identifier of a ticker with delimiter to separate base/target, eg. BTC_USDT.

## Market Data endpoints

### Order book

* This endpoint retrieves the current order book of a specific pair.

```
GET /v1/coingecko/orderbook
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ticker_id| String | YES |  A pair such as  “BTC_USDT”

**Response Example:**

```javascript
{
	"ticker_id": "BTC_USDT",
	"timestamp": "1644117569935",
	"bids": [
		["41454.51413400","14.84915000"],
		["41454.45414000","0.60265000"]],
	"asks": [
		["41462.81586700","2.53834388"],
		["41464.56604200","0.00726000"]]
}
```

**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ticker_id| String| YES | A pair such as "BTC_USDT", with delimiter between different cryptoassets.
timestamp| Integer | YES | Unix timestamp in milliseconds for when the last updated time occurred.
bids | Array | YES |The current all bids in format [price, size]
asks | Array | YES |The current all asks in format [price, size]

### Trades

* The trades endpoint is to return data on all recently completed trades for a given market pair.

```
GET /v1/coingecko/historical_trades
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ticker_id | String | YES |  A pair such as  “BTC_USDT”
type| String | NO |  To indicate nature of trade - buy/sell

**Response Example:**

```javascript
{
	"sell": [{
		"price": "41501.99",
		"type": "sell",
		"trade_id": 2022020611252380101,
		"base_volume": "0.04363700",
		"target_volume": "1811.0223376300",
		"trade_timestamp": "1644117923801"
	},
	{
		"price": "41501.43",
		"type": "sell",
		"trade_id": 2022020611251328201,
		"base_volume": "0.04823000",
		"target_volume": "2001.6139689000",
		"trade_timestamp": "1644117913282"
	}],
	"buy": [{
		"price": "41502.44",
		"type": "buy",
		"trade_id": 2022020611254180201,
		"base_volume": "0.03827800",
		"target_volume": "1588.6303983200",
		"trade_timestamp": "1644117941802"
	},
	{
		"price": "41502.43",
		"type": "buy",
		"trade_id": 2022020611253617101,
		"base_volume": "0.10564700",
		"target_volume": "4384.6072222100",
		"trade_timestamp": "1644117936171"
	}]
}
```

**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
trade_timestamp | Integer | YES | Unix timestamp in milliseconds for when the transaction occurred.
trade_id | Integer | YES | A unique ID associated with the trade for the currency pair transaction
price | Decimal | YES |Last transacted price of base currency based on given quote currency
base_volume| Decimal | YES |Transaction amount in base pair volume.
target_volume| Decimal | YES |Transaction amount in target pair volume.
type | String | YES |Used to determine the type of the transaction that was completed.buy – Identifies an ask that was removed from the order book.sell – Identifies a bid that was removed from the order book

### Ticker

* The ticker endpoint is to provide a 24-hour pricing and volume summary for each market pair available on the exchange.

```
GET /v1/coingecko/tickers
```

**Parameters:**

--

**Response Example:**

```javascript
[{
	"bid": "41528.24676000",
	"ask": "41536.56324100",
	"ticker_id": "BTC_USDT",
	"base_currency": "BTC",
	"target_currency": "USDT",
	"last_price": "41532.40",
	"base_volume": "3418.937574",
	"target_volume": "141784268.39",
	"high": "41849.28",
	"low": "40956.44"
},
{
	"bid": "3015.83944500",
	"ask": "3024.91057000",
	"ticker_id": "ETH_USDT",
	"base_currency": "ETH",
	"target_currency": "USDT",
	"last_price": "3020.28",
	"base_volume": "29713.3440",
	"target_volume": "89424727.20",
	"high": "3055.40",
	"low": "2961.37"
}]
```

**Response Content:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
ticker_id| String| YES| Identifier of a ticker with delimiter to separate base/target, eg. BTC_USDT. 
base_currency| String| YES| Symbol/currency code of base pair, eg. BTC. 
target_currency| String| YES| Symbol/currency code of target pair, eg. USDT. 
bid| Decimal| YES| Current highest bid price. 
ask| Decimal| YES| Current lowest ask price. 
last_price| Decimal| YES| Last transacted price of base currency based on given target currency. 
base_volume| Decimal| YES|24 hour trading volume in base pair volume.
target_volume| Decimal| YES| 24 hour trading volume in target pair volume.
high| Decimal| YES| Rolling 24-hours highest transaction price.
low| Decimal| YES| Rolling 24-hours lowest transaction price.
