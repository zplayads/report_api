# 广告列表

## 接口信息

url | method | 说明
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ads | GET | 获取某应用下的广告列表

## 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
advertiser_account_id | string | path | 广告主id
app_id | string | path | 应用id

## 响应
### 示例

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

### data中信息说明

名称 | 数据类型 | 说明
---|---|--
ad_id | string | 广告id
name | string | 广告名称
