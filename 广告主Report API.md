# 1. 接口接入说明

### 1.1 接口地址
若您的账户结算方式为人民币请使用此接口：https://pa-report.zplayads.com
若您的账户结算方式为美元请使用此接口：https://pa-report-en.zplayads.com

### 1.2 接入条件
* 确保账号已经审核通过
* 需要相关的Account ID和API Key，请登录ZPLAY Ads系统后点击左侧导航的个人信息，在页面的底部进行查看

### 1.3 接口风格
* 采用restfull api风格
* 以http状态码来表明请求资源的状态

### 1.4 接口限制
* 采用签名验证的方式来验证权限

### 1.5 接口的验证
广告主审核通过后,请求接口必须带有三个参数

参数 | 类型 | 位置 | 描述
---| -- | --- | --
signature | string | query | 加密签名，signature结合了广告主的API Key和请求中的timestamp参数、nonce参数。
timestamp | int | query | 时间戳，比如：1534305711，[参考地址](https://tool.lu/timestamp/)。
nonce | int | query | 随机数，必须是正整数。

**加密/校验流程如下：**
1. 将API Key、timestamp、nonce三个参数进行字典序排序
2. 将三个参数字符串拼接成一个字符串进行sha1加密
3. 广告主获得加密后的字符串可与signature对比，标识该请求合法

*检验signature的PHP示例代码：*

```php
private function checkSignature()
{
  $signature = $_GET["signature"];
  $timestamp = $_GET["timestamp"];
  $nonce = $_GET["nonce"];

  $token = api key; // 登录ZPLAY Ads系统后点击左侧导航的个人信息，在页面的底部找到API Key
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

### 1.6 响应的数据格式
* 数据格式为json
* 字段说明
    * error 错误信息，当请求有误时的提示文字
    * data 数据

### 1.7 常见的http状态码释义

http 状态码 | 释义
---|---
200 | 获取成功
400 | 请求参数有误，请检查参数
401 | 用户验证有误，请检查验证信息
500 | 后端服务错误


# 2. 应用列表

### 2.1 接口信息

url | method | 说明
---|---|--
advertiser/{advertiser_account_id}/apps | GET | 获取应用列表

### 2.2 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
advertiser_account_id | string | path | 广告主id

### 2.3 响应
#### 示例

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

#### data中信息说明

名称 | 数据类型 | 说明
---|---|--
app_id | string | 应用id
name | string | 应用名称
os | string | 操作系统


# 3. 广告列表

### 3.1 接口信息

url | method | 说明
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ads | GET | 获取某应用下的广告列表

### 3.2 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
advertiser_account_id | string | path | 广告主id
app_id | string | path | 应用id

### 3.3 响应
#### 示例

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

#### data中信息说明

名称 | 数据类型 | 说明
---|---|--
ad_id | string | 广告id
name | string | 广告名称


# 4. 广告的统计数据列表

### 4.1 接口信息

url | method | 说明
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ad/{ad_id}/stats | GET | 广告的统计数据列表

### 4.2 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
advertiser_account_id | string | path | 广告主id
app_id | string | path | 应用id
ad_id | string | path | 广告id
page | int | query | 页码 非必填，默认为1
size | int | query | 数量 非必填，默认为20
start_date | int | query | 开始日期，非必填，时间格式为：Ymd，示例：20171106
end_date | int | query | 结束日期，非必填，时间格式为：Ymd，示例：20171106
tz | int | query | UTC时区，非必填，为空时，默认值为8，即UTC+8时区。示例：-4、6 等
group_dimension | string | query | 分组维度，非必填，默认空，支持的值有：country

### 4.3 响应
#### 示例

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
                "cvr": "882.99‰",
                "cpc": 54.08,
                "cpi": 61.83
            }
        ]
    }
}
```

#### data中信息说明

名称 | 数据类型 | 说明
---|---|--
total | int | 总数
list | array | 统计数据列表

#### 统计数据列表信息说明

名称 | 数据类型 | 说明
---|---|--
date | int | 日期
ad_id | string | 广告id
imp | int | 展示
active | int | 激活
click | int | 点击
cost | float | 消耗，单位为分
cvr | string | 激活率(千分比)
cpc | float | cpc
cpi | float | cpi
country | string | 国家，如果在请求中填写了group_dimension=country则会显示，如果没有填写，则不显示
