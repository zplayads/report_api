## 1. Integration Instruction

### 1.1 Interface address
Please choose the right interface address according to the settlement currency of your account:
When settlement currency is Chinese Yuan:https://pa-report.zplayads.com
When settlement currency is United States Dollar:https://pa-report-en.zplayads.com

### 1.2 Integration prerequisites
* Account has been approved
* Account ID and API Key, please click on the Account Information link in the left menu on the ZPLAY dashboard. Your ID and API key will be shown at the bottom of this page

### 1.3 Interface style 
* restfull api
* Use http status code to indicate the status of request resources

### 1.4 Interface limitation
* Signature is used to verify permission

### 1.5 Interface Verification
Once verification is approved, advertisers need to pass 3 parameters in their requests

Parameter | Type | Position | Description
---| -- | --- | --
signature | string | query | Encrypted signature, signature combines the API Key parameters with the timestamp parameter and the nonce parameter in the request
timestamp | int | query | Unix timestamp eg. 1534305711 [reference address](http://timestamp.online/)
nonce | int | query | Random number, should be a positive integer

**Encryption/examination process:**
1.	Sort API Key, timestamp and nonce in alphabetical order
2.	Put the three parameter strings in one string to encrypt with sha1
3.	Compare the encrypted string with signature, identify the request as legitimate

*Verify the PHP sample code in signature:*

```php
private function checkSignature()
{
  $signature = $_GET["signature"];
  $timestamp = $_GET["timestamp"];
  $nonce = $_GET["nonce"];

  $token = api key; // API Key, you can get it on ZPLAY dashboard(log in and click on the Account Information link in the left menu, then you can see it at the bottom of this page)
  $tmpArr = array($token, $timestamp, $nonce);
  sort($tmpArr, SORT_STRING);
  $tmpStr = implode( $tmpArr );
  $tmpStr = sha1( $tmpStr );

  if ($tmpStr == $signature) {
    return true;
  } else {
    return false;
  }
}
```

### 1.6 Data Format of Response
* json
* Field instruction
    * Error: error message, prompt when quest is incorrect
    * Data: data related information

### 1.7 http status code and explanations

http status code | Explanation
---|---
200 | Success
400 | Request parameter error, please check parameters
401 | User verification error, please check verification information
500 | Server error


## 2. App List Interface

### 2.1 Interface Information

url | method | Description
---|---|--
advertiser/{advertiser_account_id}/apps | GET | Get Applist

### 2.2 Request information

Field | Data Type | Position | Description
---|---|--|--
advertiser_account_id | string | path | Account id

### 2.3 Response
#### Example

```json
{
    "error": "",
    "data": [
        {
            "app_id": "80183BCF-6B19-0C8D-AC00-112DA4AEF846",
            "name": "test2",
            "os": "android"
        },
        {
            "app_id": "B5022775-02DA-C2DD-1182-C68CBFE3BD21",
            "name": "test1",
            "os": "ios"
        }
    ]
}
```

#### Data Field Description

Name | Data Type | Description
---|---|--
app_id | string | App id, which you can get on ZPLAY dashboard
name | string | Application name
os | string | Operation System


## 3. Advertise campaign List Interface

### 3.1 Interface Information

url | method | Description
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ads | GET | get campaign list of a particular application

### 3.2 Request Information

Field | Data Type | Position | Description
---|---|--|--
advertiser_account_id | string | path | Account_id
app_id | string | path | App id

### 3.3 Response
#### Example

```json
{
    "error": "",
    "data": [
        {
            "ad_id": "769974F8-BBEE-989C-FC53-ECBAC158D2C0",
            "name": "test-ad",
        }
    ]
}
```

#### Data Field Description

Name | Data Type | Instruction
---|---|--
ad_id | string | Ad id
name | string | Ad name


## 4. Advertise Statistics Interface

### 4.1 Interface Information

url | method | Description
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ad/{ad_id}/stats | GET | Advertisement Statistics

### 4.2 Request Parameter

Name | Data Type | Position | Description
---|---|--|--
advertiser_account_id | string | path | Account_id
app_id | string | path | App id
ad_id | string | path | Ad id
page | int | query | page number, optional, default is 1
size | int | query | quantity, optional, default is 20
start_date | int | query | Date starts, optional, format is YMD, eg: 20171106
end_date | int | query | Date finishes, optional, format is YMD, eg: 20171106
tz | int | query | UTC Time Zone, Optional, default is 8, means UTC+8. For example, 0, 1, -8, etc
group_dimension | string | query | group by dimension, Optional, default is null, now support: country


### 4.3 Response 
#### Example

```json
{
    "error": "",
    "data": {
        "total": 1,
        "list": [
            {
                "ad_id": "DAA6BC97-14DD-6C67-523F-8219C7E79575",
                "date": 20171101,
                "country": "CA",
                "imp": 4427,
                "active": 3419,
                "click": 3909,
                "cost": 211400,
                "ctr": 0.88,
                "ecpm": 47752.43,
                "cpc": 54.08,
                "cpi": 61.83
            }
        ]
    }
}
```

#### Data Field Description

Name | Data Type | Description
---|---|--
total | int | Total amount
list | array | Application Statistics

#### Application Statistics Interface Description

Name | Data Type | Description
---|---|--
date | int | Date
ad_id | string | Ad id
imp | int | Impression 
active | int | Activate
cost | float | cost, currency unit: cent
CVR | string | Conversion Rate(‰)
ecpm | float | eCPM
cpc | float | CPC
cpi | float | CPI
country | string | This field will be shown if you request the data by country
