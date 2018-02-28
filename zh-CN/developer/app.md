# 应用列表

## 接口信息

url | method | 说明
---|---|--
developer/{developer_account_id}/apps | GET | 获取应用列表

## 请求参数

字段 | 数据类型 | 位置 | 说明
---|---|--|--
developer_account_id | string | path | 开发者id

## 响应
### 示例

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

### data中信息说明

名称 | 数据类型 | 说明
---|---|--
app_id | string | 应用id
name | string | 应用名称
os | string | 操作系统
