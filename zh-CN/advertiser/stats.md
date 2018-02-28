# 广告的统计数据列表

## 接口信息

url | method | 说明
---|---|--
advertiser/{advertiser_account_id}/app/{app_id}/ad/{ad_id}/stats | GET | 广告的统计数据列表

## 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
advertiser_account_id | string | path | 广告主id
app_id | string | path | 应用id
ad_id | string | path | 广告id
page | int | query | 页码 非必填,默认为1
size | int | query | 数量 非必填,默认为20,不可以超过50 
start_date | int | query | 开始日期,非必填,时间格式为:Ymd,示例: 20171106
end_date | int | query | 结束日期,非必填,时间格式为:Ymd,示例: 20171106

## 响应
### 示例

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

### data中信息说明

名称 | 数据类型 | 说明
---|---|--
total | int | 总数
list | array | 统计数据列表

### 统计数据列表信息说明

名称 | 数据类型 | 说明
---|---|--
date | int | 日期
ad_id | string | 广告位id
imp | int | 展示
active | int | 激活
cost | float | 消耗
ctr | string | 点击通过率
ecpm | float | ecpm
cpc | float | cpc
cpi | float | cpi
