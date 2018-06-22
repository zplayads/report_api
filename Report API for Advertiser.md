# Interface Integration Instruction

### Interface address
https://pa-report-en.zplayads.com

### Integration prerequisites
* Account has been approved
* Please contact BD manager to receive Advertiser ID and key

### Interface style 
* restfull api
* Use http status code to indicate the status of request resources

### Interface limitation
* Signature is used to verify permission
* Verification is approved, same resource is limited to request 100 times per day

### Interface Verification
Once verification is approved, advertisers need to pass 3 parameters in their requests

Parameter | Type | Position | Description
---| -- | --- | --
signature | string | query | Encrypted signature, signature combines the API key parameters with the timestamp parameter and the nonce parameter in the request
timestamp | int | query | timestamp
nonce | int | query | Random number

Encryption/examination process:
1.	Sort token, timestamp and nonce in alphabetical order
2.	Put the three parameter strings in one string to encrypt with sha1
3.	Compare the encrypted string with signature, identify the request as legitimate

Verify the PHP sample code in signature:：
```
private function checkSignature()
{
  $signature = $_GET["signature"];
  $timestamp = $_GET["timestamp"];
  $nonce = $_GET["nonce"];

  $token = TOKEN; // Via BD to get API key
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

### Data Format of Response
* json
* Field instruction
  * Error: error message, prompt when quest is incorrect
  * Data: data related information

### http status code and explanations

http status code | Explanation
---|---
200 | Success
400 | Request parameter error, please check parameters
401 | User verification error, please check verification information
403 | Access denied, possibly because of request limit
500 | Server error


# App List Interface

## Interface Information

url | method | Description
---|---|--
advertiser/{advertiser_account_id}/apps | GET | Get Applist

## Request information

Field | Data Type | Position | Description
---|---|--|--
advertiser_account_id | string | path | Account id

## Response
### Example

```
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

### Data Field Description

Name | Data Type | Description
---|---|--
app_id | string | App id, which you can get on ZPLAY Ads
name | string | Application name
os | string | Operation System


# Advertise Unit List Interface

## Interface Information

url | method | Description
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ads | GET | get ad list of a particular application

## Request Information

Field | Data Type | Position | Description
---|---|--|--
advertiser_account_id | string | path | Account_id
app_id | string | path | App id

## Response
### Example

```
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

### Data Field Description

Name | Data Type | Instruction
---|---|--
ad_id | string | Ad id
name | string | Ad name


# Advertise Statistics Interface

## Interface Information

url | method | Description
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ad/{ad_id}/stats | GET | Advertisement Statistics

## Request Parameter

Name | Data Type | Position | Description
---|---|--|--
advertiser_account_id | string | path | Account_id
app_id | string | path | App id
ad_id | string | path | Ad id
page | int | query | page number, optional, default is 1
size | int | query | quantity, optional, default is 20, maximum is 50.
start_date | int | query | Date starts, optional, format is YMD, eg: 20171106
end_date | int | query | Date finishes, optional, format is YMD, eg: 20171106
tz | int | query | Time Zone，Optional，default is 8. For example, 0, 1, -8, etc

## Response 
### Example

```
{
    "error": "",
    "data": {
        "total": 1,
        "list": [
            {
                "ad_id": "DAA6BC97-14DD-6C67-523F-8219C7E79575",
                "date": 20171101,
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

### Data Field Description

Name | Data Type | Description
---|---|--
total | int | Total amount
list | array | Application Statistics

### Application Statistics Interface Description

Name | Data Type | Description
---|---|--
date | int | Date
ad_id | string | Ad id
imp | int | Impression 
active | int | Activate
cost | float | cost 
CVR | string | Conversion Rate
ecpm | float | eCPM
cpc | float | CPC
cpi | float | CPI
