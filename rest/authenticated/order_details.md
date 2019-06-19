# Order Details

Get an order details.

### Sample Request \(Python\)

```py
import requests

url = "https://openapi.bitmart.com/v2/orders/1223181"

// timestamp is in milliseconds and the authorization header is "Bearer " + token
headers = {"X-BM-TIMESTAMP": xxx, "X-BM-AUTHORIZATION": "xxx"}

response = requests.get(url, headers=headers)

print(response.text)
```

### Sample Request \(Go\)

```go
func main() {
    access_token := get_access_token(api_key, api_secret, memo)

    URL := endpoint + "/v2/orders/1223181"

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
```

#### Request Parameter

| Parameter | Type | Description |
| :--- | :--- | :--- |
| entrust_id | path | Order id |


#### Status Type
| Status | Description |
| :--- | :--- |
| 0 | All orders |
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
| entrust_id | string | Order id |
| symbol | string | Trading pair symbol |
| timestamp | string | Order create time (in milliseconds) |
| side | string | 'buy' or 'sell' |
| price | string | Price of the order |
| fees | string | Fees of the order |
| original_amount | string | Order amount |
| executed_amount | string | Successful amount |
| remaining_amount | string | Remaining amount |





