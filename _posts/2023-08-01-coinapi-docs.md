---
title: CoinAPI.io short docs
author: paulk
date: 2023-08-01 11:33:00 +0100
categories: [Blogging ,coins, API]
tags: [coins,api]
pin: true
math: true
mermaid: true
---

## Links

- [Links](#links)
- [Get Historical OHLCV Data Using CoinAPI](#get-historical-ohlcv-data-using-coinapi)
  - [Understanding CoinAPI Endpoints](#understanding-coinapi-endpoints)
  - [Error Code Meanings](#error-code-meanings)
  - [Timeseries periods](#timeseries-periods)
  - [Examples](#examples)
    - [Best Practices](#best-practices)
- [Metadata](#metadata)
  - [List all symbols](#list-all-symbols)
    - [HTTP Request](#http-request)
    - [URL Parameters](#url-parameters)
    - [Common output variables](#common-output-variables)
    - [Symbol identifier](#symbol-identifier)
    - [Symbol types list (enumeration of `symbol_type` output variable)](#symbol-types-list-enumeration-of-symbol_type-output-variable)
    - [Additional output variables for symbol\_type = `FUTURES`](#additional-output-variables-for-symbol_type--futures)
    - [Additional output variables for symbol\_type = `PERPETUAL`](#additional-output-variables-for-symbol_type--perpetual)
    - [Additional output variables for symbol\_type = `OPTION`](#additional-output-variables-for-symbol_type--option)
    - [Additional output variables for symbol\_type = `CONTRACT`](#additional-output-variables-for-symbol_type--contract)
  - [List all assets](#list-all-assets)
    - [HTTP Request 2](#http-request-2)
    - [URL Parameters 2](#url-parameters-2)
    - [Output variables 2](#output-variables-2)
- [Metrics](#metrics)
  - [Get listing of all supported metrics](#get-listing-of-all-supported-metrics)
    - [HTTP Request 3](#http-request-3)
    - [Output variables 3](#output-variables-3)
  - [Get current snapshot of all metrics for all exchanges](#get-current-snapshot-of-all-metrics-for-all-exchanges)
    - [HTTP Request 4](#http-request-4)
    - [Output variables 4](#output-variables-4)
  - [Exchange current metrics](#exchange-current-metrics)
    - [HTTP Request 5](#http-request-5)
    - [URL Parameters 5](#url-parameters-5)
    - [Output variables 5](#output-variables-5)
  - [Current metrics for given symbol](#current-metrics-for-given-symbol)
    - [HTTP Request 6](#http-request-6)
    - [URL Parameters 6](#url-parameters-6)
- [OHLCV](#ohlcv)
  - [Historical data](#historical-data)
    - [HTTP Request 7](#http-request-7)
    - [URL Parameters 7](#url-parameters-7)
    - [Output variables 7](#output-variables-7)
- [Order book](#order-book)
  - [JSON structure](#json-structure)
    - [Output variables 8](#output-variables-8)
  - [Current data](#current-data)
    - [HTTP Request 8](#http-request-8)
    - [URL Parameters 8](#url-parameters-8)
  - [Historical data ask/bid](#historical-data-askbid)
    - [HTTP Request 9](#http-request-9)
    - [URL Parameters 9](#url-parameters-9)
- [Markdown Text formating](#markdown-text-formating)
  - [usefull emoji's](#usefull-emojis)
    - [Headings](#headings)
    - [Prompts](#prompts)
    - [Tables](#tables)
    - [webLinks](#weblinks)
    - [Video](#video)

Usefull links:

- [API key limits are here](https://docs.coinapi.io/market-data/rest-api)
- [Real-time trades stream using WebSocket with different languages](https://docs.coinapi.io/how-to-guides/real-time-trades-stream-using-websocket-with-different-languages)

## [Get Historical OHLCV Data Using CoinAPI](https://docs.coinapi.io/how-to-guides/get-historical-ohlcv-data-using-coinapi)

### Understanding CoinAPI Endpoints

CoinAPI provides several endpoints for accessing historical data, including:

- `ohlcv/period_id/history`: Provides historical OHLCV (Open, High, Low, Close, Volume) data for specified periods.
- `trades/symbol_id/history`: Returns historical trades.
- `quotes/symbol_id/history`: Provides historical quotes.

Remember, you need to replace `period_Id` and `symbol_id` with your desired period and symbol respectively.

### Error Code Meanings

- `400 Bad Request` -- There is something wrong with your request
- `401 Unauthorized` -- Your API key is wrong
- `403 Forbidden` -- Your API key doesnt't have enough privileges to access this resource
- `429 Too many requests` -- You have exceeded your API key rate limits
- `550 No data` -- You requested specific single item that we don't have at this moment

### Timeseries periods

| Time unit | Period identifiers                                              |
|:----------|:---------------------------------------------------------------:|
| Second    | 1SEC, 2SEC, 3SEC, 4SEC, 5SEC, 6SEC, 10SEC, 15SEC, 20SEC, 30SEC  |
| Minute    | 1MIN, 2MIN, 3MIN, 4MIN, 5MIN, 6MIN, 10MIN, 15MIN, 20MIN, 30MIN  |
| Hour      | 1HRS, 2HRS, 3HRS, 4HRS, 6HRS, 8HRS, 12HRS |
| Day       | 1DAY, 2DAY, 3DAY, 5DAY, 7DAY, 10DAY |

### Examples

Python

```python
!pip install requests

import requests

def fetch_ohlcv(time_start, limit):
    url = "https://rest.coinapi.io/v1/symbols?filter_symbol_id=DERIBIT_OPT_BTC_USD_230120_17000_C"
    params = {
        "period_id": "1MTH",
        "time_start": time_start,
        "limit": limit
    }
    headers = {"X-CoinAPI-Key": "C1E9E630-5A84-4740-B2E1-78F8ECE6EF78"}  # Replace with your API key

    response = requests.get(url, headers=headers, params=params)

    # Check if the response is successful
    if response.status_code == 200:
        if response.content:
            return response.json()
        else:
            print("Response is empty.")
            return None
    else:
        # Handle other HTTP status codes
        print(f"Failed to fetch data. Status code: {response.status_code}")
        return None

# Example usage with time_start and limit
time_start = "2023-03-01T00:00:00"
limit = 100  # Replace with the desired number of data points
print(fetch_ohlcv(time_start, limit))
```

Scala

```scala
!pip install requests

import requests

def fetch_ohlcv(time_start, limit):
    url = "https://rest.coinapi.io/v1/symbols?filter_symbol_id=DERIBIT_OPT_BTC_USD_230120_17000_C"
    params = {
        "period_id": "1MTH",
        "time_start": time_start,
        "limit": limit
    }
    headers = {"X-CoinAPI-Key": "C1E9E630-5A84-4740-B2E1-78F8ECE6EF78"}  # Replace with your API key

    response = requests.get(url, headers=headers, params=params)

    # Check if the response is successful
    if response.status_code == 200:
        if response.content:
            return response.json()
        else:
            print("Response is empty.")
            return None
    else:
        # Handle other HTTP status codes
        print(f"Failed to fetch data. Status code: {response.status_code}")
        return None

# Example usage with time_start and limit
time_start = "2023-03-01T00:00:00"
limit = 100  # Replace with the desired number of data points
print(fetch_ohlcv(time_start, limit))
```

#### Best Practices

- Handle rate limiting: be aware of CoinAPI's rate limits and handle 429 HTTP status codes gracefully in your application.
- Efficient data fetching: use query parameters like time_start and limit effectively to get the data you need without excess.

> Best Practices: Handle rate limiting: be aware of CoinAPI's rate limits and handle 429 HTTP status codes gracefully in your application.
> Efficient data fetching: use query parameters like time_start and limit effectively to get the data you need without excess.
{: .prompt-tip }


[Back to Top](#links)

## [Metadata](https://docs.coinapi.io/market-data/rest-api/metadata)

### List all symbols

```python
import requests
url = 'https://rest.coinapi.io/v1/symbols'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

>The above command returns JSON structured like this:

```json
[
  {
    "symbol_id": "KRAKENFTS_PERP_BTC_USD",
    "exchange_id": "KRAKENFTS",
    "symbol_type": "PERPETUAL",
    "asset_id_base": "BTC",
    "asset_id_quote": "USD",
    "asset_id_unit": "USD",
    "future_contract_unit": 1.000000000,
    "future_contract_unit_asset": "USD",
    "data_start": "2019-10-30",
    "data_end": "2021-03-03",
    "data_quote_start": "2019-10-30T16:53:10.3262317Z",
    "data_quote_end": "2021-03-03T13:51:45.6970000Z",
    "data_orderbook_start": "2019-10-30T16:53:10.3262317Z",
    "data_orderbook_end": "2020-08-05T14:37:32.0080000Z",
    "data_trade_start": "2019-10-30T16:38:52.1620000Z",
    "data_trade_end": "2021-03-03T13:46:25.7810000Z",
    "volume_1hrs": 22897091.000000000,
    "volume_1hrs_usd": 22897091.00,
    "volume_1day": 459390289.000000000,
    "volume_1day_usd": 459390289.00,
    "volume_1mth": 12875674995.000000000,
    "volume_1mth_usd": 12875674995.00,
    "price": 51266,
    "symbol_id_exchange": "pi_xbtusd",
    "asset_id_base_exchange": "XBT",
    "asset_id_quote_exchange": "USD",
    "price_precision": 0.100000000,
    "size_precision": 1.000000000
  },
  {
    "symbol_id": "POLONIEX_SPOT_LTC_USDC",
    "exchange_id": "POLONIEX",
    "symbol_type": "SPOT",
    "asset_id_base": "LTC",
    "asset_id_quote": "USDC",
    "data_start": "2018-11-20",
    "data_end": "2021-03-01",
    "data_quote_start": "2018-11-20T15:24:58.4128803Z",
    "data_quote_end": "2021-03-01T16:07:09.3475456Z",
    "data_orderbook_start": "2018-11-20T15:24:58.4128803Z",
    "data_orderbook_end": "2020-08-05T14:37:20.2695780Z",
    "data_trade_start": "2018-11-20T15:25:38.0000000Z",
    "data_trade_end": "2021-03-01T16:03:18.0000000Z",
    "volume_1hrs": 51.686458990,
    "volume_1hrs_usd": 9036.44,
    "volume_1day": 465.568863000,
    "volume_1day_usd": 81396.28,
    "volume_1mth": 22528.276384950,
    "volume_1mth_usd": 3938661.00,
    "symbol_id_exchange": "USDC_LTC",
    "asset_id_base_exchange": "LTC",
    "asset_id_quote_exchange": "USDC",
    "price_precision": 0.000000010,
    "size_precision": 0.000000010
  }
]
```

#### HTTP Request

- `GET /v1/symbols?filter_symbol_id={filter_symbol_id}&filter_exchange_id={filter_exchange_id}&filter_asset_id={filter_asset_id}`

```http
/v1/symbols?filter_symbol_id={filter_symbol_id}&filter_exchange_id={filter_exchange_id}&filter_asset_id={filter_asset_id}
```

- `GET /v1/symbols/{exchange_id}`

```http
/v1/symbols/{exchange_id}
```

#### URL Parameters

| Parameter        | Type     | Description                                                                            |
|:-----------------|----------|:---------------------------------------------------------------------------------------|
| filter_symbol_id | string[] | Comma or semicolon delimited parts of the symbol identifier used to filter the response. (optional, e.g., `BITSTAMP_` or `BINANCE_SPOT_`)  |

#### Common output variables

| Variable         | Description                                                                            |
|:-----------------|:---------------------------------------------------------------------------------------|
| symbol_id        | Our symbol identifier, see table below for format description  |
| exchange_id      | Our identifier of the exchange where symbol is traded  |
| symbol_type      | Type of the symbol. Valid values: SPOT FUTURES OPTION PERPETUAL INDEX CREDIT CONTRACT  |
| asset_id_base    | FX Spot base asset identifier, for derivatives it's contact underlying (e.g. `BTC` for BTC/USD)  |
| asset_id_quote   | FX Spot quote asset identifier, for derivatives it's contract underlying (e.g. `USD` for BTC/USD)  |
| asset_id_unit    | Identifier of the symbol unit asset used to denominate: transactions and order books volume (optional, if not provided then data is denominated in `asset_id_base`)  |
| data_start       | Date of the first data point  |
| data_end     | Date of the last data point  |
| data_quote_start      | The date and time of first quote.  |
| data_quote_end    | The date and time of last quote  |
| data_orderbook_start   | The date and time of first order book. |
| data_orderbook_end    | The date and time of last order book.  |
| data_trade_start       | The date and time of first trade. |
| data_trade_end      | The date and time of last trade.  |
| price     | The current price.  |
| symbol_id_exchange  | Exchange symbol identifier. |
| asset_id_base_exchange  | Exchange FX Spot base asset identifier  |
| asset_id_quote_exchange    | Exchange FX Spot quote asset identifier  |
|price_precision       | Current order price precision observed (a posteriori) from the market data or known (a priori) from the data source metadata.  |
| size_precision    | Current order size precision observed (a posteriori) from the market data or known (a priori) from the data source metadata.  |

> "price_precision" and "size_precision" are data precisions and are not always the same precisions used for trading eg. for the "BINANCE" exchanges.
{: .prompt-tip }

NOTE
> You should not assume that the market data will be always within the resolution provided by the "price_precision" and "size_precision". The fact that the precision values can be derived from a posterior implies the fact that this data could be delayed, also it can be changed by the data source without notice and we will immediately deliver data with the new precision while could not update the precision values in this endpoint immediately.
{: .prompt-tip }


[Back to Top](#links)

#### Symbol identifier

Our symbol identifier is created using a pattern that depends on symbol type.

| Type        | `symbol_id` pattern                                                                    |
|:------------|:---------------------------------------------------------------------------------------|
| SPOT        | `{exchange_id}_SPOT_{asset_id_base}_{asset_id_quote}`  |
| FUTURES     | `{exchange_id}_FTS_{asset_id_base}_{asset_id_quote}_{YYMMDD of future_delivery_time}`  |
| OPTION      | `{exchange_id}_OPT_{asset_id_base}_{asset_id_quote}_{YYMMDD of option_expiration_time}_{option_strike_price}_{option_type_is_call as C/P}` |
| PERPETUAL  | `{exchange_id}_PERP_{asset_id_base}_{asset_id_quote}` |
| INDEX      | `{exchange_id}_IDX_{index_id}`  |
| CREDIT     | `{exchange_id}_CRE_{asset_id_base}`  |
| CONTACT    | `{exchange_id}_COT_{contract_id}` |

#### Symbol types list (enumeration of `symbol_type` output variable)

| Type        | Name              | Description                                                                |
|:------------|:------------------|:---------------------------------------------------------------------------|
| SPOT        | FX Spot           | Agreement to exchange one asset for another one (e.g. Buy BTC for USD)  |
| FUTURES     | Futures contract  | FX Spot derivative contract where traders agree to trade fx spot at predetermined future time  |
| OPTION      | Option contract   | FX Spot derivative contract where traders agree to trade right to require buy or sell of fx spot at agreed price on exercise date |
| PERPETUAL  | Perpetual contract | FX Spot derivative contract where traders agree to trade fx spot continously without predetermined future delivery time |
| INDEX      | Index              | Statistical composite that measures changes in the economy or markets.  |
| CREDIT     | Credit/Funding     | Margin funding contract. Order book displays lending offers and borrow bids. Price represents the daily rate.  |
| CONTACT    | Contract           | Represents other types of financial instruments (e.g. spreads, interest rate swap) |

#### Additional output variables for symbol_type = `FUTURES`

| Variable             | Description                                                                            |
|:---------------------|:---------------------------------------------------------------------------------------|
| future_delivery_time | Predetermined time of futures contract delivery date in ISO 8601  |
| future_contract_unit | Contact size (eg. 10 BTC if `future_contract_unit` = `10` and `future_contract_unit_asset` = `BTC`)  |
| future_contract_unit_asset | Identifier of the asset used to denominate the contract unit  |

#### Additional output variables for symbol_type = `PERPETUAL`

| Variable             | Description                                                                            |
|:---------------------|:---------------------------------------------------------------------------------------|
| future_contract_unit | Contact size (eg. 10 BTC if `future_contract_unit` = `10` and `future_contract_unit_asset` = `BTC`) |
| future_contract_unit_asset | Identifier of the asset used to denominate the contract unit  |

#### Additional output variables for symbol_type = `OPTION`

| Variable               | Description                                                                            |
|:-----------------------|:---------------------------------------------------------------------------------------|
| option_type_is_call    | Boolean value representing option type. `true` for Call options, `false` for Put options |
| option_strike_price    | Price at which option contract can be exercised  |
| option_contract_unit   | Base asset amount of underlying spot which single option represents  |
| option_exercise_style  | Option exercise style. Can be `EUROPEAN` or `AMERICAN`  |
| option_expiration_time | Option contract expiration time in ISO 8601  |

#### Additional output variables for symbol_type = `CONTRACT`

| Variable               | Description                                                                            |
|:-----------------------|:---------------------------------------------------------------------------------------|
| contract_delivery_time | Predetermined time of contract delivery date in ISO 8601 |
| contract_unit          | Contact size (eg. 10 BTC if `future_contract_unit` = `10` and `future_contract_unit_asset` = `BTC`) |
| contract_unit_asset    | Identifier of the asset used to denominate the contract unit  |
| contract_id            | Identifier of contract by the exchange  |

[Back to Top](#links)

### List all assets

```python
import requests
url = 'https://rest.coinapi.io/v1/assets'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

>The above command returns JSON structured like this:

```json
[
  {
    "asset_id": "BTC",
    "name": "Bitcoin",
    "type_is_crypto": 1,
    "data_start": "2010-07-17",
    "data_end": "2019-11-03",
    "data_quote_start": "2014-02-24T17:43:05.0000000Z",
    "data_quote_end": "2019-11-03T17:55:07.6724523Z",
    "data_orderbook_start": "2014-02-24T17:43:05.0000000Z",
    "data_orderbook_end": "2019-11-03T17:55:17.8592413Z",
    "data_trade_start": "2010-07-17T23:09:17.0000000Z",
    "data_trade_end": "2019-11-03T17:55:11.8220000Z",
    "data_symbols_count": 22711,
    "volume_1hrs_usd": 102894431436.49,
    "volume_1day_usd": 2086392323256.16,
    "volume_1mth_usd": 57929168359984.54,
    "price_usd": 9166.207274778093436220194944
  },
  {
    "asset_id": "USD",
    "name": "US Dollar",
    "type_is_crypto": 0,
    "data_start": "2010-07-17",
    "data_end": "2019-11-03",
    "data_quote_start": "2014-02-24T17:43:05.0000000Z",
    "data_quote_end": "2019-11-03T17:54:49.3807706Z",
    "data_orderbook_start": "2014-02-24T17:43:05.0000000Z",
    "data_orderbook_end": "2019-11-03T17:55:13.1863931Z",
    "data_trade_start": "2010-07-17T23:09:17.0000000Z",
    "data_trade_end": "2019-11-03T17:55:07.7870000Z",
    "data_symbols_count": 10728,
    "volume_1hrs_usd": 9466454016.52,
    "volume_1day_usd": 221580758173.49,
    "volume_1mth_usd": 11816685174661.70,
    "price_usd": 1.0
  }
]
```

#### HTTP Request 2

- `GET /v1/assets`

```http
/v1/assets
```

- `GET /v1/assets/{asset_id}`

```http
/v1/assets/{asset_id}
```

- `GET /v1/assets?filter_asset_id={filter_asset_id}`

```http
/v1/assets?filter_asset_id={filter_asset_id}
```

#### URL Parameters 2

| Parameter        | Type     | Description                                                                            |
|:-----------------|----------|:---------------------------------------------------------------------------------------|
| filter_asset_id  | string[] | Comma or semicolon delimited asset identifiers used to filter response. (optional, eg. `BTC`;`ETH`)|

#### Output variables 2

| Variable              | Description                                                                            |
|:----------------------|:---------------------------------------------------------------------------------------|
| asset_id              | Our asset identifier. Superset of the ISO 4217 currency codes standard.  |
| name                  | Display name of the asset.  |
| type_is_crypto        | Boolean value transported as integer; `1` for cryptocurrency assets, `0` otherwise.  |
| data_quote_start      | The date and time of first quote.  |
| data_quote_end        | The date and time of last quote  |
| data_orderbook_start  | The date and time of first order book. |
| data_orderbook_end    | The date and time of last order book.  |
| data_trade_start      | The date and time of first trade. |
| data_trade_end        | The date and time of last trade.  |
| data_quote_count      | The count of quotes. |
| data_trade_count      | The count of trades.  |
| data_symbols_count    | The count of symbols for given asset.  |
| volume_1hrs_usd       | The usd volume of all symbols associated with this asset from last 1 hour rolling period.  |
| volume_1day_usd       | The usd volume of all symbols associated with this asset from last 1 day rolling period. |
| volume_1mth_usd       | The usd volume of all symbols associated with this asset from last 1 month rolling period.  |
| price_usd             | The actual usd price.  |

> Our asset identifiers are aligned with the ISO 4217 currency codes standard only for fiat money (government or law regulated currency).
{: .prompt-tip }

NOTE 2

> Properties of the output are providing aggregated information from across all symbols related to the specific asset. If you need to calculate your aggregation (e.g., limiting only the particular type of symbols), you should use /v1/symbols endpoint as a data source.
{: .prompt-tip }

[Back to Top](#links)

## [Metrics](https://docs.coinapi.io/market-data/rest-api/metrics)

Metrics are quantitative measurements used to evaluate the performance and activity of cryptocurrency exchanges. These metrics include:

1. Trading Volume: The total amount of cryptocurrency traded on an exchange within a specific time period, indicating liquidity and activity.
2. Market Depth: The level of buy and sell orders at different price levels, providing insights into liquidity and potential price impact.
3. Order Book: A record of outstanding buy and sell orders for a cryptocurrency, reflecting supply and demand dynamics.
4. Spread: The difference between the highest bid and lowest ask prices, indicating liquidity and trading costs.
5. Price Charts: Visual representations of cryptocurrency price movements over time, helping identify trends and inform trading decisions.
6. Market Cap: The total value of a cryptocurrency calculated by its price multiplied by circulating supply, reflecting relative size and value.
7. Trading Pairs: Combinations of cryptocurrencies available for trading, including volume, price, and spread for each pair.
8. User Metrics: Data on active users, new registrations, user retention, and engagement, indicating platform popularity and growth.
9. Trading Fees: Fees charged for executing trades, including fee structure, discounts, and revenue generated by the exchange.
10. Security Metrics: Measures assessing the security of an exchange, such as past incidents, user fund protection, and security audits.

These metrics assist traders and investors in evaluating market activity, liquidity, and the reliability of crypto exchanges for informed decision-making.

### Get listing of all supported metrics

```python
import requests
url = 'https://rest.coinapi.io/v1/metrics/listing'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

>The above command returns JSON structured like this:

```json
[
  {
    "metric_id": "LIQUIDATION_QUANTITY",
	"description": "The liquidation quantity metric calculates the specific amount of the asset that must be sold to settle the trader's obligations and bring their position back to a manageable level. It helps ensure that the trader's losses are covered and that their account remains in good standing."
  },
  {
    "metric_id": "DERIVATIVES_LAST_PRICE",
	"description": "The last price represents the price at which the most recent trade of the derivative contract occurred. It provides traders and investors with the latest information on the market value of the derivative, allowing them to assess the current market sentiment, track price movements, and make informed decisions regarding their derivative positions. The derivatives last price metric is crucial for monitoring real-time market conditions and managing derivative trading strategies effectively."
  }
]
```

#### HTTP Request 3

- `GET /v1/metrics/listing`

```http
/v1/metrics/listing
```

#### Output variables 3

| Variable     | Description        |
|:-------------|:-------------------|
| metric_id    | Metric identifier  |
| description  | Metric description |

### Get current snapshot of all metrics for all exchanges

```python
import requests
url = 'https://rest.coinapi.io/v1/metrics/snapshot'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

>The above command returns JSON structured like this:

```json
[
  {
    "entry_time": "2023-06-23T11:21:22.3828110Z",
    "recv_time": "2023-06-23T11:21:22.6690000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_240329_10000_P",
    "metric_id": "IV_ASK",
    "value_decimal": 0
  },
  {
    "entry_time": "2023-06-23T11:16:22.4957330Z",
    "recv_time": "2023-06-23T11:16:22.4540000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_ETH_USD_240329_1500_C",
    "metric_id": "IV_ASK",
    "value_decimal": 0
  },
  {
    "entry_time": "2023-06-23T11:24:22.6980900Z",
    "recv_time": "2023-06-23T11:24:22.7780000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_230929_25000_C",
    "metric_id": "GREEKS_VEGA",
    "value_decimal": 51.04786
  }
]
```

#### HTTP Request 4

- `GET /v1/metrics/snapshot`

```http
/v1/metrics/snapshot
```

#### Output variables 4

| Variable      | Description                               |
|:--------------|:------------------------------------------|
| entry_time    | Time in ISO 8601                          |
| recv_time     | Time in ISO 8601                          |
| exchange_id   | The identifier for the exchange.          |
| asset_id      | The identifier for the asset.             |
| symbol_id     | The identifier for the symbol.            |
| metric_id     |The identifier for the metric.             |
| value_decimal | The decimal value for the metric.         |
| value_text    | The textual representation of the value for the metric. |
| value_time    | The timestamp value for the metric.                     |

### Exchange current metrics

```python
import requests
url = 'https://rest.coinapi.io/v1/metrics/exchange/DERIBIT/current'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

#### HTTP Request 5

- `GET /v1/metrics/exchange/{exchange_id}/current`

```http
/v1/metrics/exchange/{exchange_id}/current
```

#### URL Parameters 5

| Parameter   | Type   | Description                    |
|:------------|--------|:-------------------------------|
| exchange_id | string | Our identifier of the exchange |

>The above command returns JSON structured like this:

```json
[
  {
    "entry_time": "2023-06-16T07:55:07.2787756Z",
    "recv_time": "2023-06-16T07:55:07.0010000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_230616_25750_P",
    "metric_id": "DERIVATIVES_MARK_PRICE",
    "value_decimal": 0.0086
  },
  {
    "entry_time": "2023-06-26T07:05:39.9865633Z",
    "recv_time": "2023-06-26T07:05:39.9000000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_230728_33000_P",
    "metric_id": "IV_UNDERLYING_PRICE",
    "value_decimal": 30743.29
  },
  {
    "entry_time": "2023-06-26T07:05:17.0768012Z",
    "recv_time": "2023-06-26T07:05:16.8420000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_ETH_USD_230825_2900_P",
    "metric_id": "DERIVATIVES_MARK_PRICE_IV",
    "value_decimal": 60.77
  }
]
```

#### Output variables 5

| Variable      | Description                               |
|:--------------|:------------------------------------------|
| entry_time    | Time in ISO 8601                          |
| recv_time     | Time in ISO 8601                          |
| exchange_id   | The identifier for the exchange.          |
| asset_id      | The identifier for the asset.             |
| symbol_id     | The identifier for the symbol.            |
| metric_id     |The identifier for the metric.             |
| value_decimal | The decimal value for the metric.         |
| value_text    | The textual representation of the value for the metric. |
| value_time    | The timestamp value for the metric.                     |

### Current metrics for given symbol

```python
import requests
url = 'https://rest.coinapi.io/v1/metrics/symbol/DERIBIT_OPT_BTC_USD_240329_80000_P/current'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

#### HTTP Request 6

- `GET /v1/metrics/symbol/{symbol_id}/current`

```http
/v1/metrics/symbol/{symbol_id}/current
```

#### URL Parameters 6

| Parameter | Type   | Description       |
|:----------|--------|:------------------|
| symbol_id | string | Symbol identifier |

>The above command returns JSON structured like this:

```json
[
  {
    "entry_time": "2023-06-23T12:38:48.2782698Z",
    "recv_time": "2023-06-23T12:38:48.0140000Z",
    "exchange_id": "DERIBITUAT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_240329_80000_P",
    "metric_id": "GREEKS_RHO",
    "value_decimal": -594.10357
  },
  {
    "entry_time": "2023-06-23T12:38:48.2782698Z",
    "recv_time": "2023-06-23T12:38:48.0140000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_240329_80000_P",
    "metric_id": "GREEKS_VEGA",
    "value_decimal": 49.31013
  },
  {
    "entry_time": "2023-06-23T12:38:48.2782698Z",
    "recv_time": "2023-06-23T12:38:48.0140000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_240329_80000_P",
    "metric_id": "GREEKS_THETA",
    "value_decimal": -6.14104
  },
  {
    "entry_time": "2023-06-23T12:38:48.2782698Z",
    "recv_time": "2023-06-23T12:38:48.0140000Z",
    "exchange_id": "DERIBIT",
    "symbol_id": "DERIBIT_OPT_BTC_USD_240329_80000_P",
    "metric_id": "DERIVATIVES_INDEX_PRICE",
    "value_decimal": 30027.74
  }
]
```

[Back to Top](#links)

## [OHLCV](https://docs.coinapi.io/market-data/rest-api/ohlcv)

### Historical data

```python
import requests
url = 'https://rest.coinapi.io/v1/ohlcv/BITSTAMP_SPOT_BTC_USD/history?period_id=1MIN&time_start=2016-01-01T00:00:00'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

>The above command returns JSON structured like this:

```json
[
  {
    "time_period_start": "2017-01-01T00:00:00.0000000Z",
    "time_period_end": "2017-01-02T00:00:00.0000000Z",
    "time_open": "2017-01-01T00:01:08.0000000Z",
    "time_close": "2017-01-01T23:59:46.0000000Z",
    "price_open": 966.340000000,
    "price_high": 1005.000000000,
    "price_low": 960.530000000,
    "price_close": 997.750000000,
    "volume_traded": 6850.593308590,
    "trades_count": 7815
  },
  {
    "time_period_start": "2017-01-02T00:00:00.0000000Z",
    "time_period_end": "2017-01-03T00:00:00.0000000Z",
    "time_open": "2017-01-02T00:00:05.0000000Z",
    "time_close": "2017-01-02T23:59:37.0000000Z",
    "price_open": 997.750000000,
    "price_high": 1032.000000000,
    "price_low": 990.010000000,
    "price_close": 1012.540000000,
    "volume_traded": 8167.381030180,
    "trades_count": 7871
  }
]
```

#### HTTP Request 7

- `GET /v1/ohlcv/{symbol_id}/history?period_id={period_id}&time_start={time_start}&time_end={time_end}&limit={limit}&include_empty_items={include_empty_items}`

```http
/v1/ohlcv/{symbol_id}/history?period_id={period_id}&time_start={time_start}&time_end={time_end}&limit={limit}&include_empty_items={include_empty_items}
```

#### URL Parameters 7

| Parameter   | Type   | Description                    |
|:------------|--------|:-------------------------------|
| symbol_id | string | Symbol identifier of requested timeseries (full list available `here`) |
| period_id | string | Identifier of requested timeseries period (required, e.g. `5SEC` or `2MTH`, full list `here`) |
| time_start | timestring | Timeseries starting time in ISO 8601 (required) |
| time_end | timestring | Timeseries ending time in ISO 8601 (optional, if not supplied then the data is returned to the end or when count of result elements reaches the limit) |
| include_empty_items | bool | Include items with no activity? (optional, default value is `false`, possible values are `true` or `false`) |
| limit | int | Amount of items to return (optional, mininum is 1, maximum is 100000, default value is 100, if the parameter is used then every 100 output items are counted as one request) |

#### Output variables 7

| Variable           | Description                                        |
|:-------------------|:---------------------------------------------------|
| time_period_start  | Period starting time (range left inclusive)        |
| time_period_end    | Period ending time (range right exclusive)         |
| time_open          | Time of first trade inside period range            |
| time_close         | Time of last trade inside period range             |
| price_open         | First trade price inside period range              |
| price_high         | Highest  traded price inside period range          |
| price_low          | Lowest traded price inside period range            |
| price_close        | Last trade price inside period range               |
| volume_traded      | Cumulative base amount traded inside period range  |
| trades_count       | Amount of trades executed inside period range      |

[Back to Top](#links)

## [Order book](https://docs.coinapi.io/market-data/rest-api/order-book)

This section describes calls related to order book data, also known as books or passive level 2 data.

### JSON structure

>Order book related commands return JSON structured like this:

```json
[
  {
    "symbol_id": "BITSTAMP_SPOT_BTC_USD",
    "time_exchange": "2013-09-28T22:40:50.0000000Z",
    "time_coinapi": "2017-03-18T22:42:21.3763342Z",
    "asks": [
      {
        "price": 456.35,
        "size": 123
      },
      {
        "price": 456.36,
        "size": 23
      },
      ... cut ...
    ],
    "bids": [
      {
        "price": 456.10,
        "size": 42
      },
      {
        "price": 456.09,
        "size": 5
      },
      ... cut ...
    ]
  }
]
```

#### Output variables 8

| Variable         | Description                                        |
|:-----------------|:---------------------------------------------------|
| symbol_id        | Our symbol identifier, format documented [here](https://docs.coinapi.io/market-data/rest-api/metadata/#list-all-symbols-get). |
| time_exchange    | Exchange time of order book         |
| time_coinapi     | CoinAPI time when order book received from exchange |
| asks             | Best 20 ask levels in order from best to worst      |
| bids             | Best 20 bid levels in order from best to worst      |
| price            | Price of bid/ask          |
| size             | Volume resting on bid/ask level in base amount     |

### Current data

`Latest data` is there too

```python
import requests
url = 'https://rest.coinapi.io/v1/orderbooks/current'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

#### HTTP Request 8

- `GET /v1/orderbooks/current?filter_symbol_id={filter_symbol_id}&limit_levels={limit_levels}`

```http
/v1/orderbooks/current?filter_symbol_id={filter_symbol_id}&limit_levels={limit_levels}
```

- `GET /v1/orderbooks/{symbol_id}/current?limit_levels={limit_levels}`

```http
/v1/orderbooks/{symbol_id}/current?limit_levels={limit_levels}
```

#### URL Parameters 8

| Parameter   | Type   | Description                    |
|:------------|--------|:-------------------------------|
| filter_symbol_id | string | Comma or semicolon delimited parts of symbol identifier used to filter response. (optional, full list available [here](https://docs.coinapi.io/market-data/rest-api/metadata/#list-all-symbols-get)) |
| limit_levels | int | Maximum amount of levels from each side of the book to include in response (optional) |

> When requesting current data for a specific symbol, output is not encapsulated into JSON array as only one item is returned
{: .prompt-tip }

NOTE 2

> GET /v1/orderbooks/current endpoint is charged one request per 100 data points returned after applying a filter defined by filter_symbol_id parameter.
{: .prompt-tip }

NOTE 3

> When requesting current order book data limited to a single level, then quotes are actually used. This information is important from the perspective that quotes data could be faster than order book data (behavior is dependent solely one the data source) and they can have the size equal to 0 when the size is unknown. Some data sources publish order books and separately quote data (without the sizes) at a higher frequency. In that case, we will merge the order book feed with quotes feed to make sure that our updates are as fast as possible. The quotes will have the size equal to 0 as the value is unknown and the customer can decide if these higher frequency updates without the sizes are valuable or if not then can discard them or ask for at least 2 order book levels (in case of a REST API call). For the data sources that publish order books only or order books and quotes with the sizes then this will not happen.
{: .prompt-tip }

[Back to Top](#links)

### Historical data ask/bid

```python
import requests
url = 'https://rest.coinapi.io/v1/orderbooks/BITSTAMP_SPOT_BTC_USD/history?time_start=2016-01-01T00:00:00'
headers = {'X-CoinAPI-Key' : '73034021-THIS-IS-SAMPLE-KEY'}
response = requests.get(url, headers=headers)
```

#### HTTP Request 9

- `GET /v1/orderbooks/{symbol_id}/history?time_start={time_start}&time_end={time_end}&limit={limit}&limit_levels={limit_levels}`

```http
/v1/orderbooks/{symbol_id}/history?time_start={time_start}&time_end={time_end}&limit={limit}&limit_levels={limit_levels}
```

#### URL Parameters 9

| Parameter    | Type       | Description                    |
|:-------------|------------|:-------------------------------|
| symbol_id    | string     | Symbol identifier for requested timeseries (full list available [here](https://docs.coinapi.io/market-data/rest-api/metadata/#list-all-symbols-get)) |
| time_start   | timestring | Starting time in ISO 8601 (required) |
| time_end     | timestring | Timeseries ending time in ISO 8601 (optional, if not supplied then the data is returned to the end or when result elements count reaches the limit) |
| limit_levels | int        | Maximum amount of levels from each side of the book to include in response (optional) |
| limit        | int        | Amount of items to return (optional, mininum is 1, maximum is 100000, default value is 100, if the parameter is used then every 100 output items are counted as one request) |

> The historical order book data via the REST API is currently limited by a number of updates and to the maximum number of 20 levels.
{: .prompt-tip }

[Back to Top](#links)

## Markdown Text formating

### usefull emoji's

full list can be found [Here](https://gist.github.com/rxaviers/7360908) and for markdown [here](https://github.com/ikatyang/emoji-cheat-sheet)

💥 👉 👈 👇 ☝️ 📌 📆  📈  📘 📒 📕 📗 📓 🔬 🔖 💡 📢  🔍 🔋 🔧 🎉 ⚡ ☁️ 🐍 🐼 🐌

🐳 🔥 ❗ ❓ ⭐ 💨 💦 👽 👍💪 🙈  🙉 🙊 👀 🚀 🌝 🏁 🔑 🔒 🕙 💯 ✔️ ✅ 🔗

markdown:

🪙 📎  📂

#### Headings

<h1 class="mt-5">H1 - heading</h1>

<h2 data-toc-skip>H2 - heading</h2>

<h3 data-toc-skip>H3 - heading</h3>

<h4>H4 - heading</h4>

#### Prompts

> An example showing the `tip` type prompt.
{: .prompt-tip }

> An example showing the `info` type prompt.
{: .prompt-info }

> An example showing the `warning` type prompt.
{: .prompt-warning }

> An example showing the `danger` type prompt.
{: .prompt-danger }

#### Tables

| Company                      | Contact          | Country |
|:-----------------------------|:-----------------|--------:|
| Alfreds Futterkiste          | Maria Anders     | Germany |
| Island Trading               | Helen Bennett    | UK      |
| Magazzini Alimentari Riuniti | Giovanni Rovelli | Italy   |

#### webLinks

<http://127.0.0.1:4000>

#### Video

{% include embed/youtube.html id='Balreaj8Yqs' %}

[Back to Top](#links)
