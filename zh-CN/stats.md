# 应用的统计数据列表

## 接口信息

url | method | 说明
---|---|--
developer/{developer_account_id}/app/{app_id}/stats | GET | 应用的统计数据列表

## 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
developer_account_id | string | path | 开发者id
app_id | string | path | 应用id
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

### data中信息说明

名称 | 数据类型 | 说明
---|---|--
total | int | 总数
list | array | 统计数据列表

### 统计数据列表信息说明

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
