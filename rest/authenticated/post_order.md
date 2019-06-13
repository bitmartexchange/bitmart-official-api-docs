# Place a New Order

Create a new order.

### HMAC SHA256 signature

HMAC SHA256 signature will be used as the value of _**X-BM-SIGNATURE**_ header. We highly recommend to sign the payload in case the request body is hacked. Be aware that the order of request parameters in the signed payload must be the same as the order of parameters in the post body.

Here is a sample from the Linux command line request using ```echo```, ```openssl``` and ```curl```.

| Key | Value |
| :--- | :--- |
| symbol | BMX_ETH |
| side | buy |
| amount | 1 |
| price | 1 |

**In order to protect the exchange, the bid price cannot exceed 5 times of the price of bid1 and the ask price cannot exceed 5 times of the price of ask1.**

The parameters should be sorted base on alphabetic order, which is amount, price, side and symbol. Please see the example below.

```sh
[linux]$ echo -n "amount=1&price=1&side=buy&symbol=BMX_ETH" | openssl dgst -sha256 -hmac "8c08d9d5c3d15b105dbddaf96e427ac6"
(stdin)= 2302088e474141fc7b498d0fa96c9cc2eda39a5a24fd1495d469d0a72e5fd483
```

### Sample Request \(Python 2\)

```py
import requests
import hmac
import hashlib
from urllib import urlencode

url = "https://openapi.bitmart.com/v2/orders"

// timestamp is in milliseconds and the authorization header is "Bearer " + token
// X-BM-SIGNATURE is the HMAC SHA256 signature of the request parameters encrypted by API Secret

secret = "***"
headers = {"X-BM-TIMESTAMP": ***, "X-BM-AUTHORIZATION": "***", "Content-Type": "application/json"}

// notice: the price and amount should be formatted as *.******, not scientific notation
// amount = format(amount, ".10f")
// price = format(price, ".10f")
data = {"symbol": "BMX_ETH", "amount": 1,"price": 1,"side": "buy"}

// notice: the parameters should be sorted in alphabetical order
sorted_data = sorted(data.items(), key=lambda d: d[0], reverse=False)
message = str(urlencode(sorted_data))
signed_message = hmac.new(secret, message, hashlib.sha256).hexdigest()

// if you are using python 3, it will be a little different to sign the message
// you can use: 
// signed_message = hmac.new(bytes(secret, "utf-8"), bytes(message, "utf-8"), hashlib.sha256).hexdigest()

// add signed_message to the headers
headers["X-BM-SIGNATURE"] = signed_message

data = json.dumps(data)

response = requests.post(url, data=data, headers=headers)

print(response.text)
```

### Sample Request \(Go\)
```go
func main() {
    amount := 1.0
    price := 1.0
    side := "buy"
    symbol := "BMX_ETH"

    access_token := get_access_token(api_key, api_secret, memo)

    URL := endpoint + "/v2/orders"

    jsdata, err := json.Marshal(map[string]interface{}{
        "amount": amount,
        "price":  price,
        "side":   side,
        "symbol": symbol,
    })
    if err != nil {
        panic(err)
    }

    buf := bytes.NewBuffer(jsdata)

    message := fmt.Sprintf(
        "amount=%s&price=%s&side=%s&symbol=%s",
        strconv.FormatFloat(amount, 'f', -1, 64),
        strconv.FormatFloat(price, 'f', -1, 64),
        side,
        symbol,
    )

    req, _ := http.NewRequest("POST", URL, buf)

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("X-Bm-Timestamp", fmt.Sprintf("%d", time.Now().UnixNano()/1000000))
    req.Header.Add("X-Bm-Authorization", "Bearer "+access_token)
    req.Header.Add("X-Bm-Signature", create_sha256_signature(api_secret, message))

    res, err := http.DefaultClient.Do(req)
    if err != nil {
        panic(err)
    }
    defer res.Body.Close()

    body, _ := ioutil.ReadAll(res.Body)

    fmt.Println(string(body))
}

```

### Sample Request

```js
{
   "symbol":"BMX_ETH",
   "amount":1,
   "price":1,
   "side":"buy"
}
```

### Sample Response
```js
{
   "entrust_id":1223181
}
```

#### Response Details

| Key | Type | Description |
| :--- | :--- | :--- |
| entrust_id | string | Order id |





