# List Orders

Get a list of user orders.

### Sample Request \(Python\)

```py
import requests

url = "https://openapi.bitmart.com/v2/orders?symbol=BMX_ETH&status=0&offset=0&limit=100"

// timestamp is in milliseconds and the authorization header is "Bearer " + token
headers = {"X-BM-TIMESTAMP": xxx, "X-BM-AUTHORIZATION": "xxx"}

response = requests.get(url, headers=headers)

print(response.text)
```

### Sample Request \(Go\)
```go
func main() {
    access_token := get_access_token(api_key, api_secret, memo)

    URL := endpoint + "/v2/orders?symbol=BMX_ETH&status=1&offset=0&limit=100"

    req, _ := http.NewRequest("GET", URL, nil)

    req.Header.Add("X-Bm-Timestamp", fmt.Sprintf("%d", time.Now().UnixNano()/1000000))
    req.Header.Add("X-Bm-Authorization", "Bearer "+access_token)

    res, err := http.DefaultClient.Do(req)
    if err != nil {
        panic(err)
    }
    defer res.Body.Close()

    body, _ := ioutil.ReadAll(res.Body)

    fmt.Println(string(body))
}

```

### Sample Response

```js
{
   "orders":[
      {
         "entrust_id":1223181,
         "symbol":"BMX_ETH",
         "timestamp":1528060666000,
         "side":"buy",
         "price":"1.000000",
         "fees":"0.1",
         "original_amount":"1",
         "executed_amount":"1",
         "remaining_amount":"0",
         "status":3
      }
   ],
   "total_pages":1,
   "total_orders":1,
   "current_page":0,
}
```

#### Request Parameter

| Parameter | Type | Description |
| :--- | :--- | :--- |
| symbol | query | Trading pair symbol |
| status | query | Integer enum, please check the table below |
| offset | query | Current page, which starts from 0 |
| limit | query | Maximum size of the results in one page (default 500, maximum 1000) |

#### Polling

```offset``` and ```limit``` are optional. If they are not passed in, the result contains all orders that satisfy ```symbol``` and ```status``` restrictions.

However, for high-volume trading it is strongly recommended that you maintain your own list of open orders and use one of the streaming market data feeds to keep it updated. You should poll the open orders endpoint once when you start trading to obtain the current state of any open orders.

#### Status Type
| Status | Description |
| :--- | :--- |
| 1 | Pending orders |
| 2 | Partially successful orders |
| 3 | Success orders |
| 4 | Canceled orders |
| 5 | Pending and partially successful orders |
| ~~6~~ | ~~Success and canceled orders~~ |

_Status == 6 is no longer supported._

#### Response Details

| Key | Type | Description |
| :--- | :--- | :--- |
| orders | array | List of orders |
| entrust_id | string | Order id |
| symbol | string | Trading pair symbol |
| timestamp | string | Order create time (in milliseconds) |
| side | string | 'buy' or 'sell' |
| price | string | Price of the order |
| fees | string | Fees of the order |
| original_amount | string | Order amount |
| executed_amount | string | Successful amount |
| remaining_amount | string | Remaining amount |
| total_pages | string | Total number of pages in that status |
| total_orders | string | Total number of orders in that status |
| current_page | string | Current page number |
| status | string | Integer enum, please check the table above |




