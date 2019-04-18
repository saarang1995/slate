---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  # - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the CoinDCX API. Here you'll find handy documentation about our REST and Websocket APIs.

Data resources can be accessed via standard HTTPS requests in UTF-8 format to a REST API or Websocket endpoint.


# General API Information


| Type          | Endpoint                 | Protocol  |
|---------------|--------------------------|-----------|
| REST API      | https://api.coindcx.com  | HTTPS     |
| Websocket APi | wss://stream.coindcx.com | Websocket |


<ul>
<li>
Connecting to private channels requires to provide KEY and SECRET which you can generate as follows:
<ol>
  <li>Go to your CoinDCX profile section</li>
  <li>Click `Access API dashboard`</li>
  <li>Click Create API key button and follow the process of verifications</li>
</ol>
</li>
<li>
All time and timestamp related fields are in milliseconds.
</li>
<li>
Type of Orders 
<ol>
<li>"take_profit"</li>
<li>"stop_limit"</li>
<li>"market_order"</li>
<li>"limit_order"</li>
</ol>
</li>
</ul>


<!-- <aside class="notice">The base URL for all the API calls is `https://api.coindcx.com` </aside> -->

> The python version used for API samples is 2.7

# REST API - Public

## Ticker
<!-- ### HTTP Request -->
`GET /exchange/ticker`

```javascript
const request = require('request')

var baseurl = "https://api.coindcx.com"
	
request.get(baseurl + "/exchange/ticker",function(error, response, body) {
	console.log(body);
})
```
```python
import requests # Install requests module first.

url = "https://api.coindcx.com/exchange/ticker"

response = requests.get(url)
data = response.json()
print(data)
```
> Response

```json
[
  {
    "market": "REQBTC",
    "change_24_hour": "-1.621",
    "high": "0.00002799",
    "low": "0.00002626",
    "last_price": "0.00002663",
    "bid": "0.00002663",
    "ask": "0.00002669",
    "timestamp": 1524211224
  }
]
```

### Response


| Name           | Type   | Description                             |
|----------------|--------|-----------------------------------------|
| market         | string | Market name                             |
| change_24_hour | number | Change in market price in last 24 hours |
| high           | string | Highest price offered in the orderbook  |
| low            | string | Lowest price offered in the orderbook   |
| last_price     | number | Highest ask offer in the orderbook      |
| bid            | string | Highest bid offer in the orderbook      |
| ask            | string | Highest ask offer in the orderbook      |
| timestamp      | string | Timestamp if the ticker                 |

<aside> Note: A ticker is generated once every second</aside>

## Markets
<!-- ### HTTP Request -->
`GET /exchange/v1/markets`

```javascript
const request = require('request')

var baseurl = "https://api.coindcx.com"
	
request.get(baseurl + "/exchange/v1/markets",function(error, response, body) {
	console.log(body);
})
```
```python
import requests # Install requests module first.

url = "https://api.coindcx.com/exchange/v1/markets"

response = requests.get(url)
data = response.json()
print(data)

```
> Response:

```json
[
  "SNTBTC",
  "TRXBTC",
  "TRXETH"
  .
  .
]
```

### Response
Returns an array of strings of currently active markets.


## Markets details
<!-- ### HTTP Request -->

`GET /exchange/v1/markets_details`

```javascript
const request = require('request')

var baseurl = "https://api.coindcx.com"
	
request.get(baseurl + "/exchange/v1/markets_details",function(error, response, body) {
	console.log(body);
})
```
```python
import requests # Install requests module first.

url = "https://api.coindcx.com/exchange/v1/markets_details"

response = requests.get(url)
data = response.json()
print(data)
```       
> Response:

```json
[
  {
    "coindcx_name": "SNMBTC",
    "base_currency_short_name": "BTC",
    "target_currency_short_name": "SNM",
    "target_currency_name": "Sonm",
    "base_currency_name": "Bitcoin",
    "min_quantity": 1,
    "max_quantity": 90000000,
    "min_price": 5.66e-7,
    "max_price": 0.0000566,
    "min_notional": 0.001,
    "base_currency_precision": 8,
    "target_currency_precision": 0,
    "step": 1,
    "order_types": [ "take_profit", "stop_limit", "market_order", "limit_order" ],
    "symbol": "SNMBTC",
    "ecode": "B",
    "max_leverage": 3,
    "max_leverage_short": null,
    "pair": "B-SNM_BTC",
    "status": "active"
  }
]
```

### Response


| Name                       | Type   | Description                                                                |
|----------------------------|--------|----------------------------------------------------------------------------|
| coindcx_name               | string | Currency pair name.                                                        |
| base_currency_short_name   | string | Base currency's short name.                                                |
| target_currency_short_name | string | Target currency's short name.                                              |
| target_currency_name       | string | Target currency's full name.                                               |
| base_currency_name         | string | Base currency's full name.                                                 |
| min_quantity               | number | Minimum quantity.                                                          |
| max_quantity               | number | Maximum quantity.                                                          |
| min_price                  | number | Minimum price.                                                             |
| max_price                  | number | Maximum price.                                                             |
| min_notional               | number | Minimum notional price.                                                    |
| base_currency_precision    | number | Decimal precision for the base currency.                                   |
| target_currency_precision  | number | Decimal precision for the target currency.                                 |
| step                       | number | step count.                                                                |
| order_types                | array  | Array of all the allowed order type.                                       |
| symbol                     | string | Currency pair name.                                                        |
| ecode                      | string | Exchange code from where the currency pair' data exists(ex: B for Binance) |
| max_leverage               | number | Maximum laverage allowed on the currency pair.                             |
| max_leverage_short         | number | Maximum laverage short allowed on the currency pair.                       |
| pair                       | string | Pair name of currency pair for connecting to Websocket API.                |
| status                     | string | Current status of Currency pair Wallet.                                    |

## Trades
<!-- ### HTTP request -->
`GET /exchange/v1/trades/:market`

```python
import requests # Install requests module first.

url = "https://api.coindcx.com/exchange/v1/trades/SNTBTC" # Replace 'SNTBTC' with your desired market pair.

response = requests.get(url)
data = response.json()
print(data)
```
```javascript
const request = require('request')

var baseurl = "https://api.coindcx.com"

// Replace the "SNTBTC" with the desired market pair.
request.get(baseurl + "/exchange/v1/trades/SNTBTC",function(error, response, body) {
	console.log(body);
})
```

> Response

```json
[
  {
    "p":  0.00001693,
    "q":  394,
    "T":  1521476030955.09,
    "m":  false
  }
]
```

### Parameters
| Name   | Required | Example |
|--------|----------|---------|
| market | Yes      | SNTBTC  |

### Response

This API provides with a sorted list of most recent 50 trades.

| Name | Type    | Description                               |
|------|---------|-------------------------------------------|
| p    | number  | trade price                               |
| q    | number  | quantity                                  |
| T    | number  | timestamp of trade                        |
| m    | boolean | whether the buyer is market maker or not. |




## Order book
### HTTP request
`GET /exchange/v1/books/:market`

### Path parameters
| Name   | Required | Example |
|--------|----------|---------|
| market | Yes      | SNTBTC  |

```javascript
const request = require('request')

var baseurl = "https://api.coindcx.com"

// Replace the "SNTBTC" with the desired market pair.
request.get(baseurl + "/exchange/v1/books/SNTBTC",function(error, response, body) {
	console.log(body);
})
```
```python
import requests # Install requests module first.

url = "https://api.coindcx.com/exchange/v1/books/SNTBTC" # Replace 'SNTBTC' with the desired market pair.

response = requests.get(url)
data = response.json()
print(data)

```
> Response

```json
{  
  "asks":{
    "0.00160900":"23.79000000",
    "0.00161000":"300.85",
    "0.00161400":"11.25",
    "0.00161500":"101.82",
    "0.00161700":"222.37000000"
    ,
    ,
    ,
  },
  "bids":{  
    "0.00160400":"24.24000000",
    "0.00160300":"7.63",
    "0.00160100":"917.51000000",
    "0.00159900":"40.8",
    "0.00159700":"6"
    ,
    ,
    ,
  }
```

<aside class="warning">This end point returns unsorted objects of bids and asks. They must be sorted locally at your end</aside>



### 


# Authentication

<aside class="warning">All the Authenticated API calls use POST method. Parameters are to be passed as JSON in the request body. Every request must contain a timstamp parameter of when the request was generated.</aside>

> To authorize, use this code:

```ruby
  require 'net/http'
  require 'uri'
  require 'json'
  require 'openssl'



  secret = "Your secret key"
  key = "Your API key"


  payload = {
    "side" : "buy",
    "order_type" : "limit_order",
    "price_per_unit": 0.00001724,
    "market" : "SNTBTC",
    "total_quantity" : 100,
    "timestamp": 1524211224
  }.to_json

  signature = OpenSSL::HMAC.hexdigest(OpenSSL::Digest.new('sha256'), secret, payload)


  headers = {
    'Content-Type' => 'application/json',
    'X-AUTH-APIKEY' => key, 
    'X-AUTH-SIGNATURE' => signature
  }

  uri = URI.parse("https://api.coindcx.com/exchange/v1/orders/create")

  https = Net::HTTP.new(uri.host, uri.port)
  https.use_ssl = true
  request = Net::HTTP::Post.new(uri.path, headers)

  request.body = payload

  response = https.request(request)
```

> Sample order creation with auth

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
    "side": "buy",  #Toggle between 'buy' or 'sell'.
  "order_type": "limit_order", #Toggle between a 'market_order' or 'limit_order'.
  "market": "SNTBTC", #Replace 'SNTBTC' with your desired market pair.
  "price_per_unit": 0.03244, #This parameter is only required for a 'limit_order'
  "total_quantity": 400, #Replace this with the quantity you want
  "timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/create"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)

```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


	body = {
		"side": "buy",	//Toggle between 'buy' or 'sell'.
		"order_type": "limit_order", //Toggle between a 'market_order' or 'limit_order'.
		"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
		"price_per_unit": "0.03244", //This parameter is only required for a 'limit_order'
		"total_quantity": 400, //Replace this with the quantity you want
		"timestamp": timeStamp
	}

	const payload = new Buffer(JSON.stringify(body)).toString();
	const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

	const options = {
		url: baseurl + "/exchange/v1/orders/create",
		headers: {
			'X-AUTH-APIKEY': key,
			'X-AUTH-SIGNATURE': signature
		},
		json: true,
		body: body
	}

	request.post(options, function(error, response, body) {
		console.log(body);
	})
```

> Make sure to replace API key and API secret with your own.

The authentication procedure is as follows:
<ul>
  <li>The payload is the parameters object, JSON encoded</li>
`payload = parameters-object -> JSON encode`
  <br><br>
  <li>The signature is the hex digest of an HMAC-SHA256 hash where the message is your payload, and the secret key is your API secret.</li>
`signature = HMAC-SHA256(payload, api-secret).digest('hex')`
</ul>
<br>
 <p>After this, You will have to add following headers into all the authenticated requests</p>

| Header Name      | Value        |
|------------------|--------------|
| X-AUTH-APIKEY    | your-api-key |
| X-AUTH-SIGNATURE | signature    |

<aside class="notice">
You must replace <code>your-api-key</code> and <code>signature</code> with your personal API key and generated signature respectively.
</aside>

# User

## Get balances

```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp
timeStamp = int(round(time.time() * 1000))

body = {
    "timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/users/balances"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json();
print(data);
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = ""


body = {
	"timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
	url: baseurl + "/exchange/v1/users/balances",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
}

request.post(options, function(error, response, body) {
	console.log(body);
})
```

> Response:

```json
[
  {
    "currency": "BTC",
    "balance": 1.167,
    "locked_balance": 2.1
  }
]
```

> Locked balance is the balance currently being used by an open order

This endpoint retrieves account's balances.

### HTTP Request

`POST /exchange/v1/users/balances`

<!--######################## START user info ######################## -->
## Get user info

```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp
timeStamp = int(round(time.time() * 1000))

body = {
  "timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/users/info"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json();
print(data);
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = ""


body = {
  "timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
  url: baseurl + "/exchange/v1/users/info",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
}

request.post(options, function(error, response, body) {
  console.log(body);
})
```

> Response:

```json
[
  {
    "coindcx_id": "fda259ce-22fc-11e9-ba72-ef9b29b5db2b",
    "first_name": "First name",
    "last_name": "Last name",
    "mobile_number": "000000000",
    "email": "test@coindcx.com"
  }
]
```
> coindcx_id is the user id

This endpoint retrieves user info.

### HTTP Request

`POST /exchange/v1/users/info`

<!--######################## END user info ######################## -->


# Order
Enum definitions for the purpose of order are as follows:

| Name       | Values                    |
|------------|---------------------------|
| side       | buy, sell                 |
| order_type | market_order, limit_order |
| timestamp  | 1524211224                |
| ecode      | I, B, HB                  |

## New order

```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
    "side": "buy",	#Toggle between 'buy' or 'sell'.
	"order_type": "limit_order", #Toggle between a 'market_order' or 'limit_order'.
	"market": "SNTBTC", #Replace 'SNTBTC' with your desired market pair.
	"price_per_unit": 0.03244, #This parameter is only required for a 'limit_order'
	"total_quantity": 400, #Replace this with the quantity you want
	"timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/create"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
	"side": "buy",	//Toggle between 'buy' or 'sell'.
	"order_type": "limit_order", //Toggle between a 'market_order' or 'limit_order'.
	"market": "SNTBTC", //Replace 'SNTBTC' with your desired market.
	"price_per_unit": "0.03244", //This parameter is only required for a 'limit_order'
	"total_quantity": 400, //Replace this with the quantity you want
	"timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
	url: baseurl + "/exchange/v1/orders/create",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
}

request.post(options, function(error, response, body) {
	console.log(body);
})

```

> Response:

```json
{  
   "orders":[  
     {  
        "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
        "market":"TRXETH",
        "order_type":"limit_order",
        "side":"buy",
        "status":"open",
        "fee_amount":0.0000008,
        "fee":0.1,
        "total_quantity":2,
        "remaining_quantity":2.0,
        "avg_price":0.0,
        "price_per_unit":0.00001567,
        "created_at":"2018-04-19T18:17:28.022Z",
        "updated_at":"2018-04-19T18:17:28.022Z"
     }
   ]
}
```


Use this endpoint to place a new order on the exchange

### HTTP Request

`POST /exchange/v1/orders/create`

### Parameters

| Name           | Required | Example      | Description                                    |
|----------------|----------|--------------|------------------------------------------------|
| market         | Yes      | SNTBTC       | The trading pair                               |
| total_quantity | Yes      | 1.101        | Quantity to trade                              |
| price_per_unit | No       | 0.082        | Price per unit (not required for market order) |
| side           | Yes      | buy          | Specify buy or sell                            |
| order_type     | Yes      | market_order | Order Type                                     |
| timestamp      | Yes      | 1524211224   | When was the request generated                 |

## Create multiple orders

<aside class="notice">
Multiple ordering API is only supported for CoinDCX markets. Set ecode parameter as <code>I</code> 
</aside>

```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {"orders":[
    "side": "buy",  #Toggle between 'buy' or 'sell'.
    "order_type": "limit_order", #Toggle between a 'market_order' or 'limit_order'.
    "market": "SNTBTC", #Replace 'SNTBTC' with your desired market pair.
    "price_per_unit": 0.03244, #This parameter is only required for a 'limit_order'
    "total_quantity": 400, #Replace this with the quantity you want
    "timestamp": timeStamp,
    "ecode": "I"
  ],
  [
    "side": "buy",  #Toggle between 'buy' or 'sell'.
    "order_type": "limit_order", #Toggle between a 'market_order' or 'limit_order'.
    "market": "SNTBTC", #Replace 'SNTBTC' with your desired market pair.
    "price_per_unit": 0.03244, #This parameter is only required for a 'limit_order'
    "total_quantity": 400, #Replace this with the quantity you want
    "timestamp": timeStamp,
    "ecode": "I"
  ]
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/create_multiple"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {"orders": [{
          "side": "buy",  //Toggle between 'buy' or 'sell'.
          "order_type": "limit_order", //Toggle between a 'market_order' or 'limit_order'.
          "market": "BTCINR", //Replace 'SNTBTC' with your desired market.
          "price_per_unit": "466330", //This parameter is only required for a 'limit_order'
          "total_quantity": 0.01, //Replace this with the quantity you want
          "timestamp": timeStamp,
          "ecode": "I"
        },
        {
          "side": "buy",  //Toggle between 'buy' or 'sell'.
          "order_type": "limit_order", //Toggle between a 'market_order' or 'limit_order'.
          "market": "BTCINR", //Replace 'SNTBTC' with your desired market.
          "price_per_unit": "466330", //This parameter is only required for a 'limit_order'
          "total_quantity": 0.01, //Replace this with the quantity you want
          "timestamp": timeStamp,
          "ecode": "I"
        }
      ]}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
  url: baseurl + "/exchange/v1/orders/create_multiple",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
}

request.post(options, function(error, response, body) {
  console.log(body);
})

```

> Response:

```json
{  
   "orders":[  
     {  
        "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
        "market":"TRXETH",
        "order_type":"limit_order",
        "side":"buy",
        "status":"open",
        "fee_amount":0.0000008,
        "fee":0.1,
        "total_quantity":2,
        "remaining_quantity":2.0,
        "avg_price":0.0,
        "price_per_unit":0.00001567,
        "created_at":"2018-04-19T18:17:28.022Z",
        "updated_at":"2018-04-19T18:17:28.022Z"
     }
   ]
}
```


Use this endpoint to place a multiple orders on the exchange

### HTTP Request

`POST /exchange/v1/orders/create_multiple`

### Parameters in an array of objects

| Name           | Required | Example      | Description                                    |
|----------------|----------|--------------|------------------------------------------------|
| market         | Yes      | SNTBTC       | The trading pair                               |
| total_quantity | Yes      | 1.101        | Quantity to trade                              |
| price_per_unit | No       | 0.082        | Price per unit (not required for market order) |
| side           | Yes      | buy          | Specify buy or sell                            |
| order_type     | Yes      | market_order | Order Type                                     |
| timestamp      | Yes      | 1524211224   | When was the request generated                 |
| ecode          | Yes      | I            | Exchange code                                  |

##  Order status
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
    "id": "ead19992-43fd-11e8-b027-bb815bcb14ed", # Enter your Order ID here.
	"timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/status"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
	"id": "qwd19992-43fd-14e8-b027-bb815bnb14ed", //Replace it with your Order ID.
	"timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
	url: baseurl + "/exchange/v1/orders/status",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
}

request.post(options, function(error, response, body) {
	console.log(body);
})
```

> Response:

```json 
{  
  "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
  "market":"TRXETH",
  "order_type":"limit_order",
  "side":"buy",
  "status":"open",
  "fee_amount":0.0000008,
  "fee":0.1,
  "total_quantity":2,
  "remaining_quantity":2.0,
  "avg_price":0.0,
  "price_per_unit":0.00001567,
  "created_at":"2018-04-19T18:17:28.022Z",
  "updated_at":"2018-04-19T18:17:28.022Z"
}
```


Use this endpoint to fetch status of any order

### HTTP Request

`POST /exchange/v1/orders/status`

### Parameters

| Name      | Required | Example                              | Description                    |
|-----------|----------|--------------------------------------|--------------------------------|
| id        | Yes      | ead19992-43fd-11e8-b027-bb815bcb14ed | The ID of the order            |
| timestamp | Yes      | 1524211224                           | When was the request generated |


##  Multiple order status
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
  "ids": ["8a2f4284-c895-11e8-9e00-5b2c002a6ff4", "8a1d1e4c-c895-11e8-9dff-df1480546936"]
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/status_multiple"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
  "ids": ["8a2f4284-c895-11e8-9e00-5b2c002a6ff4", "8a1d1e4c-c895-11e8-9dff-df1480546936"]
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
  url: baseurl + "/exchange/v1/orders/status_multiple",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
}

request.post(options, function(error, response, body) {
  console.log(body);
})
```

> Response:

```json 
[
  {  
    "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
    "market":"TRXETH",
    "order_type":"limit_order",
    "side":"buy",
    "status":"open",
    "fee_amount":0.0000008,
    "fee":0.1,
    "total_quantity":2,
    "remaining_quantity":2.0,
    "avg_price":0.0,
    "price_per_unit":0.00001567,
    "created_at":"2018-04-19T18:17:28.022Z",
    "updated_at":"2018-04-19T18:17:28.022Z"
  }
]
```


Use this endpoint to fetch status of any order

### HTTP Request

`POST /exchange/v1/orders/status_multiple`

### Parameters

| Name | Required | Example        | Description        |
|------|----------|----------------|--------------------|
| ids  | Yes      | ["id1", "id3"] | Array of order IDs |



##  Active orders
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
	"side": "buy", # Toggle between a 'buy' or 'sell' order.
    "market": "SNTBTC", # Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/active_orders"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
	"side": "buy", //Toggle between 'buy' or 'sell'.
	"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
	url: baseurl + "/exchange/v1/orders/active_orders",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
}

request.post(options, function(error, response, body) {
	console.log(body);
})
```

> Response:

```json 
[
  {  
    "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
    "market":"TRXETH",
    "order_type":"limit_order",
    "side":"buy",
    "status":"open",
    "fee_amount":0.0000008,
    "fee":0.1,
    "total_quantity":2,
    "remaining_quantity":2.0,
    "avg_price":0.0,
    "price_per_unit":0.00001567,
    "created_at":"2018-04-19T18:17:28.022Z",
    "updated_at":"2018-04-19T18:17:28.022Z"
  }
]
```


Use this endpoint to fetch active orders

### HTTP Request

`POST /exchange/v1/orders/active_orders`

### Parameters

| Name      | Required | Example    | Description                    |
|-----------|----------|------------|--------------------------------|
| market    | Yes      | SNTBTC     |                                |
| side      | No       | buy        |                                |
| timestamp | Yes      | 1524211224 | When was the request generated |

## Account Trade history
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
  "from_id": 352622,
  "limit": 50
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/trade_history"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
  "from_id": 352622,
  "limit": 50
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
  url: baseurl + "/exchange/v1/orders/trade_history",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
}

request.post(options, function(error, response, body) {
  console.log(body);
})
```

> Response
```json
[
  {
    "id":                         564389,
    "order_id":                   "ee060ab6-40ed-11e8-b4b9-3f2ce29cd280",
    "side":                       "buy",
    "fee_amount":                 "0.00001129",
    "ecode":                      "B",
    "quantity":                   67.9,
    "price":                      0.00008272,
    "symbol":                     "SNTBTC",
    "timestamp":                  1533700109811
  }
]
```
Use this endpoint to fetch trades associated with your account

### HTTP Request

`POST /exchange/v1/orders/trade_history`

### Parameters

| Name    | Required | Example | Description                                                                           |
|---------|----------|---------|---------------------------------------------------------------------------------------|
| limit   | No       | 100     | Default 500                                                                           |
| from_id | No       | 28473   | Trade ID after which you want the data. If not supplied, latest trades would be given |



##  Active orders count
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
	"side": "buy", # Toggle between a 'buy' or 'sell' order.
    "market": "SNTBTC", # Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/active_orders_count"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
	"side": "buy", //Toggle between 'buy' or 'sell'.
	"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
	url: baseurl + "/exchange/v1/orders/active_orders_count",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
}

request.post(options, function(error, response, body) {
	console.log(body);
})
```

> Response:

```json 
 { count: 1, status: 200 }
```


Use this endpoint to fetch active orders count

### HTTP Request

`POST /exchange/v1/orders/active_orders_count`

### Parameters

| Name      | Required | Example    | Description                    |
|-----------|----------|------------|--------------------------------|
| market    | Yes      | SNTBTC     |                                |
| side      | No       | buy        |                                |
| timestamp | Yes      | 1524211224 | When was the request generated |


##  Cancel all
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
	"side": "buy", # Toggle between a 'buy' or 'sell' order.
    "market": "SNTBTC", # Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/cancel_all"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


	body = {
		"side": "buy", //Toggle between 'buy' or 'sell'. Not compulsory
		"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
		"timestamp": timeStamp
	}

	const payload = new Buffer(JSON.stringify(body)).toString();
	const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

	const options = {
		url: baseurl + "/exchange/v1/orders/cancel_all",
		headers: {
			'X-AUTH-APIKEY': key,
			'X-AUTH-SIGNATURE': signature
		},
		json: true,
		body: body
	}

	request.post(options, function(error, response, body) {
		console.log(body);
	})
```

> Response:

```json 

```


Use this endpoint to cancel multiple active orders in a single API call

### HTTP Request

`POST /exchange/v1/orders/cancel_all`

### Parameters

| Name      | Required | Example    | Description                    |
|-----------|----------|------------|--------------------------------|
| market    | Yes      | SNTBTC     |                                |
| side      | No       | buy        |                                |
| timestamp | Yes      | 1524211224 | When was the request generated |

Sending side param is optional. You may cancel all the sell orders of SNTBTC by sending
<br>
<code>{market: "SNTBTC", side  : "sell"}</code>


Or you may cancel all your orders in SNTBTC market by sending
<br>
<code>{market: "SNTBTC"}</code>


##  Cancel  multiple By Ids
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
  "ids": ["8a2f4284-c895-11e8-9e00-5b2c002a6ff4", "8a1d1e4c-c895-11e8-9dff-df1480546936"]
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/cancel_by_ids"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


  body = {
    ids: ["8a2f4284-c895-11e8-9e00-5b2c002a6ff4", "8a1d1e4c-c895-11e8-9dff-df1480546936"]
  }

  const payload = new Buffer(JSON.stringify(body)).toString();
  const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

  const options = {
    url: baseurl + "/exchange/v1/orders/cancel_by_ids",
    headers: {
      'X-AUTH-APIKEY': key,
      'X-AUTH-SIGNATURE': signature
    },
    json: true,
    body: body
  }

  request.post(options, function(error, response, body) {
    console.log(body);
  })
```

> Response:

```json 

```


Use this endpoint to cancel multiple active orders in a single API call

### HTTP Request

`POST /exchange/v1/orders/cancel_by_ids`

### Parameters

| Name | Required | Example        | Description        |
|------|----------|----------------|--------------------|
| ids  | Yes      | ["id1", "id3"] | Array of order IDs |



##  Cancel
```ruby

```

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one, you can generate it from CoinDCX website.
key = ""
secret = ""

# Generating a timestamp.
timeStamp = int(round(time.time() * 1000))

body = {
    "id": "ead19992-43fd-11e8-b027-bb815bcb14ed", # Enter your Order ID here.
	"timestamp": timeStamp
}

json_body = json.dumps(body, separators = (',', ':'))

signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

url = "https://api.coindcx.com/exchange/v1/orders/cancel"

headers = {
    'Content-Type': 'application/json',
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
}

response = requests.post(url, data = json_body, headers = headers)
data = response.json()
print(data)
```

```shell

```

```javascript
const request = require('request')
const crypto = require('crypto')

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
	"id": "ead19992-43fd-11e8-b027-bb815bcb14ed", //Replace this with your Order ID.
	"timestamp": timeStamp
}

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

const options = {
	url: baseurl + "/exchange/v1/orders/cancel",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
}

request.post(options, function(error, response, body) {
	console.log(body);
})

```

> Response:

```json 

```


Use this endpoint to cancel an active orders

### HTTP Request

`POST /exchange/v1/orders/cancel`

### Parameters

| Name      | Required | Example                              | Description                    |
|-----------|----------|--------------------------------------|--------------------------------|
| id        | Yes      | ead19992-43fd-11e8-b027-bb815bcb14ed | The ID of the order            |
| timestamp | Yes      | 1524211224                           | When was the request generated |









<!-- ------------------- START Sockets ---------------------- -->

# Sockets
<aside class="notice">Sockets are currently available only for the INR market.</aside>

## PUBLIC

<h3>To connect to public socket</h3>

Refer to the right panel.

```python
import socketio

socketEndpoint = 'wss://stream.coindcx.com'
sio = socketio.Client()

sio.connect(socketEndpoint, transports = 'websocket')
sio.emit('join', { 'channelName': 'channelName' })

# Listen update on channelName
@sio.on('channelName')
def on_message(response):
    print(response.data)

# leave a channel
sio.emit('leave', { 'channelName' : channelName })

```

```javascript
import io from 'socket.io-client';

const socketEndpoint = "wss://stream.coindcx.com";

const socket = io(socketEndpoint, {
  transports: ['websocket']
});


//Join Channel
socket.emit('join', {
  'channelName': "channelName",
});

//Listen update on channelName
socket.on('eventName', (response) => {
  console.log(response.data);
});

// leave a channel
socket.emit('leave', {
  'channelName': channelName
});
```

## Order book

### Definitions
<ul>
  <li><strong>Channel: </strong>use 'pair' from Markets details API response(Example: B-SNM_BTC,B-XRP_ETH, HB-SWM_BTC).</li>
  <li><strong>Event: </strong>depth-update</li>
</ul>


### Response
<ul>
  <li>a stand for asks</li>
  <li>b stand for bids</li>
</ul>

```python
@sio.on('depth-update')
def on_message(response):
    print(response.data.a) # asks
    print(response.data.b) # bids
```

```javascript
socket.on("depth-update", (response) => {
  console.log(response.data.a); //asks
  console.log(response.data.b); //bids
});
```

> Order book response:

```json
{
  "a": [["251005.00000000", 0.019]],
  "b": [["251009.00000000", 0.029]]
}
```

## Trades

### Definitions
<ul>
  <li><strong>Channel: </strong>use 'pair' from Markets details API response(Example: B-SNM_BTC,B-XRP_ETH, HB-SWM_BTC). </li>
  <li><strong>Event: </strong>new-trade</li>
</ul>


### Response
<ul>
  <li>m stands for whether the buyer is market maker or not</li>
  <li>p is the trade price</li>
  <li>q is the quantity</li>
  <li>T is the timestamp of trade</li>
  <li>s is the symbol(currency)</li>
</ul>

```python
@sio.on('new-trade')
def on_message(response):
  print(response.data)
```

```javascript
socket.on("new-trade", (response) => {
  console.log(response.data);
});
```

> Trade response:

```json
{
  "T": 1545896665076.92,
  "p": 0.9634e-4,
  "q": 0.1e1,
  "s": "XRPBTC",
  "m": true
}
```

## ACCOUNT

<h3>To connect to the account socket</h3>

Get your API key and Secret by simply following these steps:
<ul>
  <li>Go to the profile section on CoinDCX</li>
  <li>Click on `Access API dashboard`</li>
  <li>Click on `Create API key` and follow the process of verification</li>
</ul>

Refer to the right panel.

```python

import socketio
import hmac
import hashlib
import json
socketEndpoint = 'wss://stream.coindcx.com'
sio = socketio.Client()

sio.connect(socketEndpoint, transports = 'websocket')

secret = 'secret'
key = 'key'

body={"channel":"coindcx"}
json_body=json.dumps(body, separators = (',', ':'))
signature=hmac.new(secret, json_body, digestmod=hashlib.sha256).hexdigest()

# Join channel
sio.emit('join', { 'channelName': 'coindcx', 'authSignature': signature, 'apiKey' : key })

# Listen update on eventName
@sio.on('eventName')
def on_message(response):
    print(response.data)

# leave a channel
sio.emit('leave', { 'channelName' : 'coindcx' })

```

```javascript

import io from 'socket.io-client';
const socketEndpoint = "wss://stream.coindcx.com";

//connect to server.
const socket = io(socketEndpoint, {
  transports: ['websocket']
});

const secret = "secret";
const key = "key";


const body = { channel: "coindcx" };
const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex')

//Join channel
socket.emit('join', {
  'channelName': "coindcx",
  'authSignature': signature,
  'apiKey' : key
});


//Listen update on eventName
socket.on("eventName", (response) => {
  console.log(response.data);
});


// In order to leave a channel
socket.emit('leave', {
  'channelName': 'coindcx'
});
```

## Balances

### Definitions
<ul>
  <li><strong>Channel:</strong> coindcx</li>
  <li><strong>Event:</strong> balance-update</li>
</ul>


### Response
<ul>
  <li>balance is the usable balance</li>
  <li>Locked balance is the balance currently being used by an open order</li>
  <li>currency is the currency like LTC, BTC etc.</li>
</ul>

```python
@sio.on('balance-update')
def on_message(response):
  if response.event == 'balance-update':
    print(response.data)
```

```javascript
socket.on("balance-update", (response) => {
  if (response.event == "balance-update") {
    console.log(response.data);
  }
});
```

> Balance update response:

```json
{
  "balance": "1000.00000000",
  "locked_balance": "1.00000000",
  "currency": "XRP"
}
```

## Trades

### Definitions
<ul>
  <li><strong>Channel:</strong> coindcx</li>
  <li><strong>Event:</strong> trade-update</li>
</ul>

### Response
<ul>
  <li>o is client order id / system generated order id</li>
  <li>t is trade id</li>
  <li>s is symbol/market (LTCBTC)</li>
  <li>p is price</li>
  <li>q is quantity</li>
  <li>T is timestamp</li>
  <li>m stands for whether the buyer is market maker or not.</li>
  <li>f is fee amount</li>
  <li>e is exchange identifier</li>
  <li>x is status</li>
</ul>

```python
@sio.on('trade-update')
def on_message(response):
    print(response.data)
```

```javascript
socket.on("trade-update", (response) => {
  console.log(response.data);
});
```


> Trade update response:

```json
[{
  "o": "28c58ee8-09ab-11e9-9c6b-8f2ae34ea8b0",
  "t": "17105",
  "s": "XRPBTC",
  "p": "0.00009634",
  "q": "1.0",
  "T": 1545896665076.92,
  "m": true,
  "f": "0.000000009634",
  "e": "I",
  "x": "filled"
}]
```
<!-- ------------------- END Sockets ---------------------- -->

<!-- 

# API call limits
We have rate limits in place to facilitate availability of our resources to a wider set of people. Typically you can place around 4 orders per second. The exact number depends on the server load.
In aggregate, you may call `https//api.coindcx.com` not more than 10 times per second. -->
