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
开发者审核通过后,请求接口必须带有三个参数

参数 | 类型 | 位置 | 描述
---| -- | --- | --
signature | string | query | 加密签名，signature结合了开发者的API Key和请求中的timestamp参数、nonce参数。
timestamp | int | query | 时间戳，比如：1534305711，[参考地址](https://tool.lu/timestamp/)。
nonce | int | query | 随机数，必须是正整数。

**加密/校验流程如下：**
1. 将API Key、timestamp、nonce三个参数进行字典序排序
2. 将三个参数字符串拼接成一个字符串进行sha1加密
3. 开发者获得加密后的字符串可与signature对比，标识该请求合法

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
    * error 错误信息,当请求有误时的提示文字
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
developer/{developer_account_id}/apps | GET | 获取应用列表

### 2.2 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
developer_account_id | string | path | 开发者id

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


# 3. 广告位列表

### 3.1 接口信息

url | method | 说明
---|---|--
developer/{developer_account_id}/app/{app_id}/ad_units | GET | 获取某应用下的广告位列表

### 3.2 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
developer_account_id | string | path | 开发者id
app_id | string | path | 应用id

### 3.3 响应
#### 示例

```json
{
    "error": "",
    "data": [
        {
            "app_id": "B5022775-02DA-C2DD-1182-C68CBFE3BD21",
            "ad_unit_id": "769974F8-BBEE-989C-FC53-ECBAC158D2C0",
            "name": "test-adplace",
            "ad_type": "4"
        }
    ]
}
```

#### data中信息说明

名称 | 数据类型 | 说明
---|---|--
app_id | string | 应用id
ad_unit_id | string | 广告位id
name | string | 广告位名称
ad_type | enum | 广告类型：1=>插屏，4=>视频


# 4. 应用的统计数据列表

### 4.1 接口信息

url | method | 说明
---|---|--
developer/{developer_account_id}/app/{app_id}/stats | GET | 应用的统计数据列表

### 4.2 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
developer_account_id | string | path | 开发者id
app_id | string | path | 应用id
page | int | query | 页码 非必填，默认为1
size | int | query | 数量 非必填，默认为20 
start_date | int | query | 开始日期，非必填，时间格式为：Ymd，示例：20171106
end_date | int | query | 结束日期，非必填，时间格式为：Ymd，示例：20171106
tz | int | query | 时区，非必填，默认8，示例：-4、6 等
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
                "date": 20171005,
                "app_id": "80183BCF-6B19-0C8D-AC00-112DA4AEF846",
                "ad_unit_id": "818D83B1-20D6-EEC4-47C9-A6E6CCB3A17A",
                "country": "CA",
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

#### data中信息说明

名称 | 数据类型 | 说明
---|---|--
total | int | 总数
list | array | 统计数据列表

#### 统计数据列表信息说明

名称 | 数据类型 | 说明
---|---|--
date | int | 日期
app_id | string | 应用id
ad_unit_id | string | 广告位id
play_start | int | 播放开始
play_finish | int | 播放结束
imp | int | 展示
click | int | 点击
request | int | 请求
income | int | 收益, 单位为分
country | string | 国家，如果在请求中填写了group_dimension=country则会显示，如果没有填写，则不显示
