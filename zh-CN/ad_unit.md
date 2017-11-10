# 广告位列表

## 接口信息

url | method | 说明
---|---|--
developer/{developer_account_id}/app/{app_id}/ad_units | GET | 获取某应用下的广告位列表

## 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
developer_account_id | string | path | 开发者id
app_id | string | path | 应用id

## 响应
### 示例

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

### data中信息说明

名称 | 数据类型 | 说明
---|---|--
ad_unit_id | string | 广告位id
app_id | string | 应用id
name | string | 广告位名称
