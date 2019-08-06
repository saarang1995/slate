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
    "volume": "10.40",
    "last_price": "0.00002663",
    "bid": "0.00002663",
    "ask": "0.00002669",
    "timestamp": 1524211224
  }
]
```

### Response


| Name           | Type   | Description                               |
|----------------|--------|-------------------------------------------|
| market         | string | Uniquely identifiable currency pair name. |
| change_24_hour | number | Change in market price in last 24 hours.  |
| high           | string | Highest market price in last 24 hours.    |
| low            | string | Lowest market price in last 24 hours.     |
| volume         | string | Volume of market in last 24 hours.        |
| last_price     | number | Latest market price.                      |
| bid            | string | Highest bid offer in the orderbook.       |
| ask            | string | Lowest ask offer in the orderbook.        |
| timestamp      | string | Timestamp.                                |

<aside> Note: A ticker response is generated every second.</aside>

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
  ,
  ,
]
```

### Response
Returns an array of strings of currently active markets.

| Name | Type  | Description                       |
|------|-------|-----------------------------------|
| --   | array | array of currently active markets |

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


| Name                       | Type   | Description                                                           |
|----------------------------|--------|-----------------------------------------------------------------------|
| coindcx_name               | string | Uniquely identifiable currency pair name.                             |
| base_currency_short_name   | string | Base currency's short name.                                           |
| target_currency_short_name | string | Target currency's short name.                                         |
| target_currency_name       | string | Target currency's full name.                                          |
| base_currency_name         | string | Base currency's full name.                                            |
| min_quantity               | number | Minimum quantity.                                                     |
| max_quantity               | number | Maximum quantity.                                                     |
| min_price                  | number | Minimum price.                                                        |
| max_price                  | number | Maximum price.                                                        |
| min_notional               | number | Minimum notional price.                                               |
| base_currency_precision    | number | Decimal precision for the base currency.                              |
| target_currency_precision  | number | Decimal precision for the target currency.                            |
| step                       | number | allowed multiple of quantity while placing order.                     |
| order_types                | array  | Array of all the active order type.                                   |
| symbol                     | string | Uniquely identifiable currency pair name.                             |
| ecode                      | string | External exchange identifier. (ex: B for Binance)                     |
| max_leverage               | number | Maximum leverage allowed for a long order.                            |
| max_leverage_short         | number | Maximum leverage allowed for a short order.                           |
| pair                       | string | Uniquely identifiable currency pair name including the exchange code. |
| status                     | string | Current status.                                                       |

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


| Name | Type    | Description                               |
|------|---------|-------------------------------------------|
| p    | number  | trade price                               |
| q    | number  | quantity                                  |
| T    | number  | timestamp of trade                        |
| m    | boolean | whether the buyer is market maker or not. |

<aside> Note: Response is a sorted list of most recent 50 trades.</aside>



## Order book
<!-- ### HTTP request -->
`GET /exchange/v1/books/:market`

### Parameters
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

### Response


| Name | Type   | Description            |
|------|--------|------------------------|
| asks | object | key-value pair of asks |
| bids | object | key-value pair of bids |

<aside class="warning">Warning: This end point returns unsorted objects of bids and asks. They must be sorted at your end</aside>

### 


# REST API - Private


<!-- > To authorize, use this code: -->

```ruby
require 'net/http'
require 'uri'
require 'json'
require 'openssl'
# Enter your API Key and Secret here. If you don't have one,
# you can generate it from the website.
key = ""  # API KEY
secret = "" # API SECRET

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

<!-- > Sample order creation with auth -->

```python
import hmac
import hashlib
import base64
import json
import time
import requests

# Enter your API Key and Secret here. If you don't have one,
# you can generate it from the website.
key = ""  # API KEY
secret = "" # API SECRET

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

// Enter your API Key and Secret here. If you don't have one,
// you can generate it from the website.
key = ""  // API KEY
secret = "" // API SECRET

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


<!-- > Make sure to replace API key and API secret with your own. -->

<aside class="warning">All the Authenticated API calls use POST method. Parameters are to be passed as JSON in the request body. Every request must contain a timestamp parameter of when the request was generated.</aside>



<ul>
  <li>
    Generate KEY and SECRET by following below mentioned authentication procedure.
    <ol>
      <li>Go to your CoinDCX profile section</li>
      <li>Click Access API dashboard</li>
      <li>Click Create API key button and follow the process of verifications</li>
    </ol>
  </li>
  <li>Create payload object using the parameters and then encode it to JSON</li>
  `payload = parameters-object -> JSON encode`
    <br><br>
  <li>Finally generate the signature, which is the hex digest of an HMAC-SHA256 hash.
      Where the message is the payload, and the api-secret is your API secret.</li>
      `signature = HMAC-SHA256(payload, api-secret).digest('hex')`
</ul>
<br>

 <p>Now, add following headers into all the authentication requests</p>

| Header Name      | Value        |
|------------------|--------------|
| X-AUTH-APIKEY    | your-api-key |
| X-AUTH-SIGNATURE | signature    |

<aside class="notice">Note:
You must replace <code>your-api-key</code> and <code>signature</code> with your personal API key and generated signature respectively.
</aside>

<!-- ## User -->
## Get balances
`POST /exchange/v1/users/balances`

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
data = response.json()
print(data)
```

```shell
```

```javascript
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Place your API key and secret below. You can generate it from the website.
key = "";
secret = "";


body = {
	"timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

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
### Response

| Name           | Type   | Description           |
|----------------|--------|-----------------------|
| currency       | string | target currency name. |
| balance        | number | balance.              |
| locked_balance | number | locked balance.       |

<aside class="notice">Note: Locked balance is the balance currently being used by an open order. </aside>
<!-- > Locked balance is the balance currently being used by an open order -->

<!-- This endpoint retrieves account's balances. -->


<!--######################## START user info ######################## -->
## Get user info

`POST /exchange/v1/users/info`

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
data = response.json()
print(data)
```

```shell
```

```javascript
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";

body = {
  "timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
  url: baseurl + "/exchange/v1/users/info",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
};

request.post(options, function(error, response, body) {
  console.log(body);
});
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

### Response

| Name          | Type   | Description                       |
|---------------|--------|-----------------------------------|
| coindcx_id    | string | User's Coindcx unique identifier. |
| first_name    | string | User's first name.                |
| last_name     | string | User's last name.                 |
| mobile_number | string | User's mobile number.             |
| email         | string | User's email id.                  |

<aside class="notice">Note: coindcx_id is the user id. </aside>
<!-- > coindcx_id is the user id -->



<!--######################## END user info ######################## -->

## Order
Enum definitions for the purpose of order are as follows:

| Name       | Values                    |
|------------|---------------------------|
| side       | buy, sell                 |
| order_type | market_order, limit_order |
| timestamp  | 1524211224                |
| ecode      | I, B, HB                  |

## New order

Endpoint to place a new order on the exchange

`POST /exchange/v1/orders/create`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


body = {
	"side": "buy",	//Toggle between 'buy' or 'sell'.
	"order_type": "limit_order", //Toggle between a 'market_order' or 'limit_order'.
	"market": "SNTBTC", //Replace 'SNTBTC' with your desired market.
	"price_per_unit": "0.03244", //This parameter is only required for a 'limit_order'
	"total_quantity": 400, //Replace this with the quantity you want
	"timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
	url: baseurl + "/exchange/v1/orders/create",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
};

request.post(options, function(error, response, body) {
	console.log(body);
});

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



### Parameters

| Name           | Required | Example      | Description                                    |
|----------------|----------|--------------|------------------------------------------------|
| market         | Yes      | SNTBTC       | The trading pair                               |
| total_quantity | Yes      | 1.101        | Quantity to trade                              |
| price_per_unit | No       | 0.082        | Price per unit (not required for market order) |
| side           | Yes      | buy          | Specify buy or sell                            |
| order_type     | Yes      | market_order | Order Type                                     |
| timestamp      | Yes      | 1524211224   | When was the request generated                 |


### Response


| Name               | Type   | Description                       |
|--------------------|--------|-----------------------------------|
| id                 | string | User's Coindcx unique identifier. |
| market             | string | currency pair name.               |
| order_type         | string | Order type to place.              |
| side               | string | Buy or Sell.                      |
| status             | string | Status of order.                  |
| fee_amount         | number | Fee charged on the order.         |
| fee                | number | Fee percentage.                   |
| total_quantity     | number | Total Quantity of order.          |
| remaining_quantity | number | Remaining quantity.               |
| avg_price          | number | Average price.                    |
| price_per_unit     | number | Price per unit of.                |
| created_at         | string | Time when the order was created.  |
| updated_at         | string | Time when the order was updated.  |

## Create multiple orders
Endpoint to place a multiple orders on the exchange

`POST /exchange/v1/orders/create_multiple`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
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
      ]};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
  url: baseurl + "/exchange/v1/orders/create_multiple",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
};

request.post(options, function(error, response, body) {
  console.log(body);
});

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


### Parameters (array of objects)

| Name           | Required | Example      | Description                                    |
|----------------|----------|--------------|------------------------------------------------|
| market         | Yes      | SNTBTC       | The trading pair                               |
| total_quantity | Yes      | 1.101        | Quantity to trade                              |
| price_per_unit | No       | 0.082        | Price per unit (not required for market order) |
| side           | Yes      | buy          | Specify buy or sell                            |
| order_type     | Yes      | market_order | Order Type                                     |
| timestamp      | Yes      | 1524211224   | When was the request generated                 |
| ecode          | Yes      | I            | Exchange code                                  |


### Response


| Name               | Type   | Description                       |
|--------------------|--------|-----------------------------------|
| id                 | string | User's Coindcx unique identifier. |
| market             | string | currency pair name.               |
| order_type         | string | Order type to place.              |
| side               | string | Buy or Sell.                      |
| status             | string | Status of order.                  |
| fee_amount         | number | Fee charged on the order.         |
| fee                | number | Fee percentage.                   |
| total_quantity     | number | Total Quantity of order.          |
| remaining_quantity | number | Remaining quantity.               |
| avg_price          | number | Average price.                    |
| price_per_unit     | number | Price per unit of.                |
| created_at         | string | Time when the order was created.  |
| updated_at         | string | Time when the order was updated.  |


<aside class="notice">Note: Multiple ordering API is only supported for CoinDCX markets. Set ecode parameter as <code>I</code> 
</aside>


##  Order status

Endpoint to fetch status of any order

`POST /exchange/v1/orders/status`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com"

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


body = {
	"id": "qwd19992-43fd-14e8-b027-bb815bnb14ed", //Replace it with your Order ID.
	"timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
	url: baseurl + "/exchange/v1/orders/status",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
};

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


### Parameters

| Name      | Required | Example                              | Description                    |
|-----------|----------|--------------------------------------|--------------------------------|
| id        | Yes      | ead19992-43fd-11e8-b027-bb815bcb14ed | The ID of the order            |
| timestamp | Yes      | 1524211224                           | When was the request generated |



### Response


| Name               | Type   | Description                       |
|--------------------|--------|-----------------------------------|
| id                 | string | User's Coindcx unique identifier. |
| market             | string | currency pair name.               |
| order_type         | string | Order type to place.              |
| side               | string | Buy or Sell.                      |
| status             | string | Status of order.                  |
| fee_amount         | number | Fee charged on the order.         |
| fee                | number | Fee percentage.                   |
| total_quantity     | number | Total Quantity of order.          |
| remaining_quantity | number | Remaining quantity.               |
| avg_price          | number | Average price.                    |
| price_per_unit     | number | Price per unit of.                |
| created_at         | string | Time when the order was created.  |
| updated_at         | string | Time when the order was updated.  |




##  Multiple order status


Endpoint to fetch status of any order

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/status_multiple`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


body = {
  "ids": ["8a2f4284-c895-11e8-9e00-5b2c002a6ff4", "8a1d1e4c-c895-11e8-9dff-df1480546936"]
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
  url: baseurl + "/exchange/v1/orders/status_multiple",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
};

request.post(options, function(error, response, body) {
  console.log(body);
});
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


### Parameters

| Name | Required | Example        | Description        |
|------|----------|----------------|--------------------|
| ids  | Yes      | ["id1", "id3"] | Array of order IDs |


### Response

| Name               | Type   | Description                       |
|--------------------|--------|-----------------------------------|
| id                 | string | User's Coindcx unique identifier. |
| market             | string | currency pair name.               |
| order_type         | string | Order type to place.              |
| side               | string | Buy or Sell.                      |
| status             | string | Status of order.                  |
| fee_amount         | number | Fee charged on the order.         |
| fee                | number | Fee percentage.                   |
| total_quantity     | number | Total Quantity of order.          |
| remaining_quantity | number | Remaining quantity.               |
| avg_price          | number | Average price.                    |
| price_per_unit     | number | Price per unit of.                |
| created_at         | string | Time when the order was created.  |
| updated_at         | string | Time when the order was updated.  |


##  Active orders
Use this endpoint to fetch active orders

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/active_orders`


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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


body = {
	"side": "buy", //Toggle between 'buy' or 'sell'.
	"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
	url: baseurl + "/exchange/v1/orders/active_orders",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
};

request.post(options, function(error, response, body) {
	console.log(body);
});
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


### Parameters

| Name      | Required | Example    | Description                     |
|-----------|----------|------------|---------------------------------|
| market    | Yes      | SNTBTC     | coindcx market name.            |
| side      | No       | buy        | side buy or sell.               |
| timestamp | Yes      | 1524211224 | When was the request generated. |


### Response

| Name               | Type   | Description                       |
|--------------------|--------|-----------------------------------|
| id                 | string | User's Coindcx unique identifier. |
| market             | string | currency pair name.               |
| order_type         | string | Order type to place.              |
| side               | string | Buy or Sell.                      |
| status             | string | Status of order.                  |
| fee_amount         | number | Fee charged on the order.         |
| fee                | number | Fee percentage.                   |
| total_quantity     | number | Total Quantity of order.          |
| remaining_quantity | number | Remaining quantity.               |
| avg_price          | number | Average price.                    |
| price_per_unit     | number | Price per unit of.                |
| created_at         | string | Time when the order was created.  |
| updated_at         | string | Time when the order was updated.  |


## Account Trade history
Endpoint to fetch trades associated with your account

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/trade_history`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


body = {
  "from_id": 352622,
  "limit": 50
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
  url: baseurl + "/exchange/v1/orders/trade_history",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
};

request.post(options, function(error, response, body) {
  console.log(body);
});
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


### Parameters

| Name    | Required | Example | Description                                                                           |
|---------|----------|---------|---------------------------------------------------------------------------------------|
| limit   | No       | 100     | Default 500                                                                           |
| from_id | No       | 28473   | Trade ID after which you want the data. If not supplied, latest trades would be given |



### Response


| Name       | Type   | Description                                     |
|------------|--------|-------------------------------------------------|
| id         | string | id.                                             |
| order_id   | string | Order id of trade.                              |
| side       | string | Buy or Sell.                                    |
| fee_amount | string | Fee charged on the order.                       |
| ecode      | string | Exchange of code.                               |
| quantity   | number | Total Quantity of order.                        |
| price      | number | Price of currency.                              |
| symbol     | string | Symbol of current paige.                        |
| timestamp  | number | Timestamp at which the last trade was completed |




##  Active orders count

Endpoint to fetch active orders count

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/active_orders_count`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


body = {
	"side": "buy", //Toggle between 'buy' or 'sell'.
	"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
	"timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
	url: baseurl + "/exchange/v1/orders/active_orders_count",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
};

request.post(options, function(error, response, body) {
	console.log(body);
});
```

> Response:

```json 
 { "count": 1, "status": 200 }
```

### Parameters

| Name      | Required | Example    | Description                    |
|-----------|----------|------------|--------------------------------|
| market    | Yes      | SNTBTC     |                                |
| side      | No       | buy        |                                |
| timestamp | Yes      | 1524211224 | When was the request generated |

### Response

| Name   | Type   | Description          |
|--------|--------|----------------------|
| count  | number | Active order counts. |
| status | number | Active status.       |

##  Cancel all
Endpoint to cancel multiple active orders.

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/cancel_all`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


	body = {
		"side": "buy", //Toggle between 'buy' or 'sell'. Not compulsory
		"market": "SNTBTC", //Replace 'SNTBTC' with your desired market pair.
		"timestamp": timeStamp
	};

	const payload = new Buffer(JSON.stringify(body)).toString();
	const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

	const options = {
		url: baseurl + "/exchange/v1/orders/cancel_all",
		headers: {
			'X-AUTH-APIKEY': key,
			'X-AUTH-SIGNATURE': signature
		},
		json: true,
		body: body
	};

	request.post(options, function(error, response, body) {
		console.log(body);
	});
```

> Response:

```json 

```

### Parameters

| Name      | Required | Example    | Description                    |
|-----------|----------|------------|--------------------------------|
| market    | Yes      | SNTBTC     |                                |
| side      | No       | buy        |                                |
| timestamp | Yes      | 1524211224 | When was the request generated |

<ul>
  <li>
  Sending side param is optional. You may cancel all the sell orders of SNTBTC by sending
  <br>
  <code>{market: "SNTBTC", side  : "sell"}</code>
  </li>
  <li>
  Or you may cancel all your orders in SNTBTC market by sending
  <br>
  <code>{market: "SNTBTC"}</code>
  </li>
</ul>


##  Cancel  multiple By Ids

Endpoint to cancel multiple active orders in a single API call

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/cancel_by_ids`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from the website.
key = "";
secret = "";


  body = {
    ids: ["8a2f4284-c895-11e8-9e00-5b2c002a6ff4", "8a1d1e4c-c895-11e8-9dff-df1480546936"]
  };

  const payload = new Buffer(JSON.stringify(body)).toString();
  const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

  const options = {
    url: baseurl + "/exchange/v1/orders/cancel_by_ids",
    headers: {
      'X-AUTH-APIKEY': key,
      'X-AUTH-SIGNATURE': signature
    },
    json: true,
    body: body
  };

  request.post(options, function(error, response, body) {
    console.log(body);
  });
```

<!-- > Response:

```json 

``` -->

### Parameters

| Name | Required | Example        | Description        |
|------|----------|----------------|--------------------|
| ids  | Yes      | ["id1", "id3"] | Array of order IDs |


##  Cancel
Endpoint to cancel an active orders

<!-- ### HTTP Request -->

`POST /exchange/v1/orders/cancel`

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
const request = require('request');
const crypto = require('crypto');

var baseurl = "https://api.coindcx.com";

var timeStamp = Math.floor(Date.now());
// To check if the timestamp is correct
console.log(timeStamp);

// Enter your API Key and Secret here. If you don't have one, you can generate it from CoinDCX website.
key = "";
secret = "";


body = {
	"id": "ead19992-43fd-11e8-b027-bb815bcb14ed", //Replace this with your Order ID.
	"timestamp": timeStamp
};

const payload = new Buffer(JSON.stringify(body)).toString();
const signature = crypto.createHmac('sha256', secret).update(payload).digest('hex');

const options = {
	url: baseurl + "/exchange/v1/orders/cancel",
	headers: {
		'X-AUTH-APIKEY': key,
		'X-AUTH-SIGNATURE': signature
	},
	json: true,
	body: body
};

request.post(options, function(error, response, body) {
	console.log(body);
});

```
<!-- 
> Response:

```json 

``` -->

### Parameters

| Name      | Required | Example                              | Description                    |
|-----------|----------|--------------------------------------|--------------------------------|
| id        | Yes      | ead19992-43fd-11e8-b027-bb815bcb14ed | The ID of the order            |
| timestamp | Yes      | 1524211224                           | When was the request generated |



<!-- ------------------- START Sockets ---------------------- -->

# Sockets
<!-- <aside class="notice">Sockets are currently available only for the INR market.</aside> --> 
Use websocket as transport.

## Public

Steps to connect to a public channel:
<ul>
  <li>Create an instance of socket by providing <code>"wss://stream.coindcx.com"</code> as an endpoint.</li>
  <li>Emit <code>'join'</code> event and provide <code>'channelName'</code> retrieved from market_details response <code>('pair')</code> as a key value pair to which you would like to connect to.</li>
  <li>Emit <code>'leave'</code> event and provide <code>'channelName'</code> as a key value pair from which you would like to disconnect.</li>
</ul>

### Parameters

| Name        | Required | Example | Description |
|-------------|----------|---------|-------------|
| channelName | Yes      | BTCUSDT |             |

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


### Parameters

| Name        | Required | Example | Description                                                                             |
|-------------|----------|---------|-----------------------------------------------------------------------------------------|
| channelName | Yes      | BTCUSDT | use 'pair' from Markets details API response(Example: B-SNM_BTC,B-XRP_ETH, HB-SWM_BTC). |

### Response


| Name | Type   | Description |
|------|--------|-------------|
| a    | string | asks.       |
| b    | string | bids.       |


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


### Parameters

| Name        | Required | Example   | Description                                                                             |
|-------------|----------|-----------|-----------------------------------------------------------------------------------------|
| channelName | Yes      | BTCUSDT   | use 'pair' from Markets details API response(Example: B-SNM_BTC,B-XRP_ETH, HB-SWM_BTC). |
| eventName   | Yes      | new-trade | New trade event.                                                                        |

### Response


| Name | Type    | Description                               |
|------|---------|-------------------------------------------|
| m    | boolean | whether the buyer is market maker or not. |
| p    | number  | trade price.                              |
| q    | number  | quantity.                                 |
| T    | number  | timestamp of trade.                       |
| s    | string  | the symbol(currency).                     |

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

## Account

Connect to the account websocket using the KEY and SECRET. If not generated yet, then please follow the steps in the 'General API information'.

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


### Parameters

| Name        | Required | Example        | Description                                                                             |
|-------------|----------|----------------|-----------------------------------------------------------------------------------------|
| channelName | Yes      | BTCUSDT        | use 'pair' from Markets details API response(Example: B-SNM_BTC,B-XRP_ETH, HB-SWM_BTC). |
| eventName   | Yes      | balance-update | Balance updates event.                                                                  |

### Response


| Name           | Type   | Description                                        |
|----------------|--------|----------------------------------------------------|
| balance        | string | the usable balance.                                |
| locked_balance | string | the balance currently being used by an open order. |
| currency       | string | the target currency like LTC, BTC etc.             |

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

### Parameters

| Name        | Required | Example      | Description                                                                             |
|-------------|----------|--------------|-----------------------------------------------------------------------------------------|
| channelName | Yes      | BTCUSDT      | use 'pair' from Markets details API response(Example: B-SNM_BTC,B-XRP_ETH, HB-SWM_BTC). |
| eventName   | Yes      | trade-update | trade updates event.                                                                    |

### Response


| Name | Type   | Description                                  |
|------|--------|----------------------------------------------|
| o    | string | client order id / system generated order id. |
| t    | string | trade id.                                    |
| s    | string | symbol/market (LTCBTC).                      |
| p    | string | price.                                       |
| q    | string | quantity.                                    |
| T    | number | timestamp.                                   |
| m    | string | whether the buyer is market maker or not.    |
| f    | string | fee amount.                                  |
| e    | string | exchange identifier.                         |
| x    | string | status.                                      |


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
