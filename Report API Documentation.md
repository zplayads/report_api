## 1. Integration Instructions

### 1.1 Request URL
https://pa-report.zplayads.com

### 1.2 Requirements
* Account approved
* Developer id and key, please contact our BD manager to get them

### 1.3 Interface Style
* restfull api
* Use http status code to indicate the status of request resources

### 1.4 Interface limit
* Signature is used to verify permission
* After verification, the same resource is limited to request 100 times a day

### 1.5 Interface Verification
After verification is approved, developers need to pass 3 parameters in their requests

Parameter | Type | Position | Description
---| -- | --- | --
signature | string | query | Encrypted signature， signature combines the API key parameters with the timestamp parameter in the request, the nonce parameter
timestamp | int | query | timestamp
nonce | int | query | Random number

Encryption/examination process:
1. Sort token, timestamp and nonce in alphabetical order
2. Put the three parameter strings in one string to encrypt with sha1
3. Compare the encrypted string with signature, identify the request as legitimate

Check the PHP sample code in signature:
```
private function checkSignature()
{
  $signature = $_GET["signature"];
  $timestamp = $_GET["timestamp"];
  $nonce = $_GET["nonce"];	

  $token = TOKEN; // API Key, you can get it on ZPLAY Ads
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
  * error: error message, prompt when quest is incorrect
  * data: data related information
  
### 1.7 http status code and explanations

http status code | explanation
---|---
200 | Success
400 | Request parameter error, please check parameters
401 | User verification error, please check verification information
403 | Access denied, possibly because of request limit
500 | Server error

## 2. App List Interface

### 2.1 Interface Information

url | method | Description
---|---|--
developer/{developer_account_id}/apps | GET | Get Applist

### 2.2 Request information

Field | Dta type | Position | Description
---|---|--|--
developer_account_id | string | path | Developer id

### 2.3 Response
#### Sample

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

#### 2.4 Data Instroction

Name | Data type | Instruction
---|---|--
app_id | string | App id, you can get App id on ZPLAY Ads
name | string | App Name
os | string | Operation System 



## 3. Ad Unit List Interface

### 3.1 Interface Information

URL | method | Description
---|---|--
developer/{developer_account_id}/app/{app_id}/ad_units | GET | get ad unit list of an application

### 3.2 Request Information

Field | Data Type | Posotion | Description
---|---|--|--
developer_account_id | string | path | Developer id
app_id | string | path | App id

### 3.3 Response
#### Sample

```
{
    "error": "",
    "data": [
        {
            "app_id": "B5022775-02DA-C2DD-1182-C68CBFE3BD21",
            "place_id": "769974F8-BBEE-989C-FC53-ECBAC158D2C0",
            "name": "test-adplace"
        }
    ]
}
```

#### Data Field Instruction

Name | Data Type | Description
---|---|--
ad_unit_id | string | Ad Unit id, you can get Ad Unit id on ZPLAY Ads 
app_id | string | App id
name | string | Ad Unit Name



## 4. Application Statistics Interface

### 4.1 Interface Information

url | method | Description
---|---|--
developer/{developer_account_id}/app/{app_id}/stats | GET | Application Statistics 

### 4.2 请求参数

Field | Data Type | 位置 | Description
---|---|--|--
developer_account_id | string | path |  Developer id
app_id | string | path | App id
page | int | query | Page number, optional, defaults 1
size | int | query | The number of data, optional, Defaults 20, no more than 50 
start_date | int | query | Start Date, optional, time format: Ymd, Sample: 20171106
end_date | int | query | End Date, optional, time format:Ymd, Sample: 20171106

### 4.3 Response
#### Sample

```
{
    "error": "",
    "data": {
        "total": 1,
        "list": [
            {
                "date": 20171005,
                "app_id": "80183BCF-6B19-0C8D-AC00-112DA4AEF846",
                "ad_unit_id": "818D83B1-20D6-EEC4-47C9-A6E6CCB3A17A",
                "play_start": 3386,
                "play_finish": 2956,
                "imp": 4095,
                "click": 3527,
                "request": 4740,
                "income": 306972
            }
        ]
    }
}
```

#### Data Feild Instruction

Name | Data Type | Description
---|---|--
total | int | total
list | array | Application Statistics

#### Application Statistics Interface Instruction

Name | Data Type | Description
---|---|--
date | int | Date
app_id | string | App id
ad_unit_id | string | Ad Unit id
play_start | int | Start
play_finish | int | End
imp | int | Impression
click | int | Click
request | int | Request
income | int | Revenue, current: cent

