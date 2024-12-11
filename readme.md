# API documentation for using SMS-BUS.COM service

API documentation for quick integration of the [“SMS BUS”](https://sms-bus.com) service for the handling of temporary virtual numbers and long-term leased virtual numbers. Ready-made examples for easy access.

API is a protocol between your software and our server. The API is needed in order to automate the process of receiving SMS messages on your side To work with the API, you must use your API Token, you can get it by going to the profile page.

**Protocol Description**

<p>All requests should go to https://sms-bus.com/api/control</p>

All requests must have an [API Token](https://sms-bus.com/profile) as a parameter "token"

## Affiliate Programs for Developers

You can get a commission on every spend when users use your program, all you need to do is sign up for an account and put you referral code on every request. [Check out for more details](https://sms-bus.com/zh/profile/referral).

Your software must be public so that everyone can buy or rent it.

## Query balance

<p>GET - https://sms-bus.com/api/control/get/balance?token=$token</p>

**Parameters**

| Field | Location | Type   | Required | Description  |
|-------|----------|--------|----------|--------------|
| token | query    | String | Yes      | Your API Token |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": {
    "frozen": 0,
    "balance": 2.52
  }
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
```

## Get all countries

<p>
GET - https://sms-bus.com/api/control/list/countries?token=$token
</p>

**Parameters**

| Field | Location | Type   | Required | Description  |
|-------|----------|--------|----------|--------------|
| token | query    | String | Yes      | Your API Token |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": {
    "1": {
      "id": 1,
      "title": "Unite State of America",
      "code": "us"
    },
    "2": {
      "id": 2,
      "title": "Russia",
      "code": "ru"
    }
  }
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
```

## Get all projects

<p>
GET - https://sms-bus.com/api/control/list/projects?token=$token
</p>

**Parameters**

| Field | Location | Type   | Required | Description  |
|-------|----------|--------|----------|--------------|
| token | query    | String | Yes      | Your API Token |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": {
    "1": {
      "id": 1,
      "title": "Telegram",
      "code": "tg"
    },
    "2": {
      "id": 2,
      "title": "Paypal",
      "code": "pp"
    }
  }
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
```

## Check project prices and available numbers

<p>
GET - https://sms-bus.com/api/control/list/prices?token=$token&country_id=$country_id
</p>

**Parameters**

| Field      | Location | Type    | Required | Description  |
|------------|----------|---------|----------|--------------|
| token      | query    | String  | Yes      | Your API Token |
| country_id | query    | Integer | Yes      | Country      |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": {
    "5": {
      "country_id": 5,
      "project_id": 2,
      "cost": 0.9,
      "total_count": 1023,
      "title": "United States of America",
      "code": "us"
    }
  }
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
{
  "code": 50001,
  "message": "No service available"
}
```

## Get a number

<p>
GET - https://sms-bus.com/api/control/get/number?token=$token&country_id=$country_id&project_id=$project_id
</p>

**Parameters**

| Field      | Location | Type    | Required | Description      |
|------------|----------|---------|----------|------------------|
| token      | query    | String  | Yes      | Your API Token     |
| country_id | query    | Integer | Yes      | Country          |
| project_id | query    | Integer | Yes      | Service          |
| refer_id   | query    | String  | No       | Pass Referral ID |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": {
    "request_id": 124,
    "number": "17548003793"
  }
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
{
  "code": 50002,
  "message": "No number available"
}
{
  "code": 50104,
  "message": "The number of waiting requests exceeds the limit, please complete or cancel it and try again."
}
{
  "code": 50201,
  "message": "Balance not enough"
}
{
  "code": 50208,
  "message": "This account is not activated, please click the activation link in the mailbox to activate"
}
```

---

### Get sms

<p>
GET - https://sms-bus.com/api/control/get/sms?token=$token&request_id=$request_id
</p>

**Parameters**

| Field      | Location | Type   | Required | Description  |
|------------|----------|--------|----------|--------------|
| token      | query    | String | Yes      | Your API Token |
| request_id | query    | String | Yes      | Request Id   |
 
**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": "429916"
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
{
  "code": 50101,
  "message": "Not received sms yet"
}
{
  "code": 50102,
  "message": "Number has been released or timeout, please reacquire"
}
```

## Cancel a request

<p>
GET - https://sms-bus.com/api/control/cancel?token=$token&request_id=$request_id
</p>

**Parameters**

| Field      | Location | Type   | Required | Description  |
|------------|----------|--------|----------|--------------|
| token      | query    | String | Yes      | Your API Token |
| request_id | query    | String | Yes      | Request Id   |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success"
}
```

Possible Errors

```json
{
  "code": 401,
  "message": "Wrong token!"
}
{
  "code": 50103,
  "message": "Can't change request status because the request has already closed."
}
```

## Reuse number for certain project

Not every number can be reused. And this feature is only available if you have previously used the number to receive SMS
from this service.

<p>
GET - https://sms-bus.com/api/control/reuse?token=$token&country_id=$country_id&project_id=$project_id&mobile_number=$mobile_number
</p>

**Parameters**

| Field         | Location | Type    | Required | Description               |
|---------------|----------|---------|----------|---------------------------|
| token         | query    | String  | Yes      | Your API Token              |
| country_id    | query    | Integer | Yes      | Country                   |
| project_id    | query    | Integer | Yes      | Service                   |
| mobile_number | query    | String  | Yes      | Number without the + sign |
| refer_id      | query    | String  | No       | Pass Referral ID          |

**Result**

Success

```json
{
  "code": 200,
  "message": "Operation Success",
  "data": {
    "request_id": 124,
    "number": "17548003793"
  }
}
```

Possible Errors

```json
{
  "code": 400,
  "message": "Wrong token!"
}
{
  "code": 50001,
  "message": "No service available."
}
{
  "code": 50007,
  "message": "The number doesn't exist."
}
{
  "code": 50107,
  "message": "The number cannot be reused."
}
{
  "code": 50108,
  "message": "The number cannot be reused."
}
{
  "code": 50109,
  "message": "The number is expired."
}
{
  "code": 50201,
  "message": "Balance not enough"
}
{
  "code": 50208,
  "message": "This account is not activated, please click the activation link in the mailbox to activate"
}
```

## Country List

| Country name                           | Country id |
|----------------------------------------|------------|
| Italy                                  | 88         |
| United Kingdom                         | 25         |
| New Zealand                            | 69         |
| Germany                                | 48         |
| Spain                                  | 190        |
| Saudi Arabia                           | 191        |
| United Arab Emirates                   | 192        |
| Israel                                 | 193        |
| Palestine                              | 194        |
| Turkey                                 | 195        |
| Qatar                                  | 196        |
| Japan                                  | 197        |
| Austria                                | 198        |
| Lithuania                              | 199        |
| Nicaragua                              | 210        |
| Cambodia                               | 211        |
| Mexico                                 | 212        |
| Singapore                              | 158        |
| Vietnam                                | 10         |
| Lao People's Democratic Republic       | 32         |
| Romania                                | 11         |
| Canada                                 | 13         |
| Thailand                               | 57         |
| India                                  | 14         |
| Bangladesh                             | 183        |
| South Africa                           | 184        |
| Morocco                                | 185        |
| Myanmar                                | 186        |
| Tajikistan                             | 187        |
| Russia                                 | 1          |
| Australia                              | 188        |
| Netherlands                            | 189        |
| Colombia                               | 200        |
| Serbia                                 | 201        |
| Ukraine                                | 4          |
| Cyprus                                 | 202        |
| United States of America               | 5          |
| Latvia                                 | 203        |
| Malaysia                               | 6          |
| Bolivia                                | 204        |
| Indonesia                              | 7          |
| Panama                                 | 205        |
| Philippines                            | 8          |
| Denmark                                | 206        |
| France                                 | 80         |
| Georgia                                | 207        |
| Cameroon                               | 208        |
| Benin                                  | 209        |