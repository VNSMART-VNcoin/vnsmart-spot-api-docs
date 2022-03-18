- [General API Information](#general-api-information)
- [Error Codes](#error-codes)
- [Public API Endpoints](#public-api-endpoints)
  - [Reference Data endpoints](#reference-data-endpoints)
    - [Exchange information](#exchange-information)
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

### Exchange information

* Current exchange trading rules and symbol information.

```
GET /coinchase/v1/public/exchangeInfo
```

**Parameters:**

--

**Response Example:**

```javascript
{
    "serverTime": 1647582556763,
    "pairs": [
        {
            "name": "BTC/USDT",
            "status": true,
            "base_currency": "BTC",
            "quote_currency": "USDT",
            "base_currency_precision": 8,
            "quote_currency_precision": 8,
            "maker_fee": "0.002",
            "taker_fee": "0.002",
            "min_price": "0.01",
            "max_price": "100000",
            "min_volume": "0.000001",
            "max_volume": "100000",
            "issue_price": "8000",
            "issue_time": 1517328000
        },
        {
            "name": "ETH/USDT",
            "status": true,
            "base_currency": "ETH",
            "quote_currency": "USDT",
            "base_currency_precision": 8,
            "quote_currency_precision": 8,
            "maker_fee": "0.002",
            "taker_fee": "0.002",
            "min_price": "0.01",
            "max_price": "1000000",
            "min_volume": "0.00001",
            "max_volume": "100000",
            "issue_price": "2.22",
            "issue_time": 1563120000
        }
    ],
    "timezone": "+08"
}
```
## Market Data endpoints

### Order book

* This endpoint retrieves the current order book of a specific pair.

```
GET /coinchase/v1/public/orderbook
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair| String | YES |  A pair such as  “BTC/USDT”
limit| Int | NO |Default 10 Valid limits:[10, 100]

**Response Example:**

```javascript
{
    "bids": [
        [
            "40770.08258400",
            "0.11603550"
        ],
        [
            "40770.07258500",
            "0.10000000"
        ],
        [
            "40769.08268400",
            "0.01000000"
        ],
        [
            "40769.04268800",
            "0.00240000"
        ],
        [
            "40768.35275700",
            "0.00010000"
        ],
        [
            "40768.03278900",
            "0.10000000"
        ],
        [
            "40766.55293700",
            "0.00500000"
        ],
        [
            "40766.53293900",
            "0.10000000"
        ],
        [
            "40765.09308300",
            "0.00052700"
        ],
        [
            "40765.03308900",
            "0.10000000"
        ]
    ],
    "asks": [
        [
            "40784.30802300",
            "0.17274550"
        ],
        [
            "40786.33822600",
            "0.00272800"
        ],
        [
            "40786.34822700",
            "0.12472500"
        ],
        [
            "40786.73826600",
            "0.69050000"
        ],
        [
            "40787.28832100",
            "0.09999950"
        ],
        [
            "40787.51834400",
            "0.03291550"
        ],
        [
            "40787.65835800",
            "0.01226350"
        ],
        [
            "40787.74836700",
            "0.03678050"
        ],
        [
            "40787.99839200",
            "0.00500000"
        ],
        [
            "40788.31842400",
            "0.04905300"
        ]
    ]
}

```

### Trades

* The trades endpoint is to return data on all recently completed trades for a given market pair.

```
GET /coinchase/v1/public/trades
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | String | YES |  A pair such as  “BTC/USDT”
limit| String | NO |  Default 100; max 500.

**Response Example:**

```javascript
[
    {
        "price": "40798.18",
        "initiative": 2,
        "id": 2022031813534101501,
        "volume": "0.15185700",
        "created_at": "1647582821015"
    },
    {
        "price": "40801.86",
        "initiative": 2,
        "id": 2022031813533751701,
        "volume": "0.14859100",
        "created_at": "1647582817517"
    }
]

```
### Ticker

* The ticker endpoint is to provide a 24-hour pricing and volume summary for each market pair available on the exchange.

```
GET /coinchase/v1/public/ticker/24hr
```

**Parameters:**

--

**Response Example:**

```javascript
{
    "ELF/USDT": {
        "base_full_name": "ELF",
        "current_price": "0.3286",
        "start_price": "0.3191",
        "volume": "144612.1119",
        "amount": "48249.5719",
        "high": "0.3406",
        "low": "0.3185",
        "count": 666,
        "sort": 30
    },
    "MDS/USDT": {
        "base_full_name": "MDS",
        "current_price": "0.000861",
        "start_price": "0.000855",
        "volume": "264444.66",
        "amount": "222.461601",
        "high": "0.000861",
        "low": "0.000840",
        "count": 20,
        "sort": 31
    },
    "HT/USDT": {
        "base_full_name": "HuoBi Token",
        "current_price": "9.0552",
        "start_price": "9.0614",
        "volume": "147428.4914",
        "amount": "1335929.6830",
        "high": "9.1165",
        "low": "9.0046",
        "count": 12036,
        "sort": 17
    }
}

``` 
