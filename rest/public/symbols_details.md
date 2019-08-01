# Symbols Details

Get the details of all symbols.

### Sample Request \(Python\)

```py
import requests

url = "https://openapi.bitmart.com/v2/symbols_details"

response = requests.request("GET", url)

print(response.text)
```

### Sample Response

```js
[
    {
        "id":"BMX_ETH",
        "base_currency":"BMX",
        "quote_currency":"ETH",
        "quote_increment":"0.1",
        "base_min_size":"0.1",
        "base_max_size":"100000000",
        "price_min_precision":6,
        "price_max_precision":8,
        "expiration":"NA"
    },
    ...
]
```

#### Response Details

| Key | Type | Description |
| :--- | :--- | :--- |
| id | string | Trading pair id |
| base\_currency | string | Base currency |
| quote\_currency | string | Quote currency |
| quote\_increment | string | Minimum order quantity as well as the quantity increment |
| base\_min\_size | string | Minimum trade volume |
| base\_max\_size | string | Maximum trade volume |
| price\_min\_precision | number | Minimum price precision (digit) used to query price and kline |
| price\_max\_precision | number | Maximum price precision (digit) used to query price and kline |
| expiration | string | Expiration date for limited contracts/pairs |



