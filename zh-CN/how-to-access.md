# 接口接入说明

### 接口地址
https://pa-report.zplayads.com

### 接入条件
* 确保账号已经审核通过
* 需要相关的开发者id和密钥, 请联系商务合作经理获取

### 接口风格
* 采用restfull api风格
* 以http状态码来表明请求资源的状态

### 接口限制
* 采用签名验证的方式来验证权限
* 验证通过后, 每天同一资源可请求100次

### 接口的验证
开发者审核通过后,请求接口必须带有三个参数

参数 | 类型 | 位置 | 描述
---| -- | --- | --
signature | string | query | 加密签名，signature结合了开发者的密钥参数和请求中的timestamp参数、nonce参数。
timestamp | int | query | 时间戳
nonce | int | query | 随机数

加密/校验流程如下：
1. 将token、timestamp、nonce三个参数进行字典序排序
2. 将三个参数字符串拼接成一个字符串进行sha1加密
3. 开发者获得加密后的字符串可与signature对比，标识该请求合法

检验signature的PHP示例代码：
```
private function checkSignature()
{
  $signature = $_GET["signature"];
  $timestamp = $_GET["timestamp"];
  $nonce = $_GET["nonce"];	

  $token = TOKEN; // 通过商务合作经理申请的密钥
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

### 响应的数据格式
* 数据格式为json
* 字段说明
  * error 错误信息,当请求有误时的提示文字
  * data 数据
  
### 常见的http状态码释义

http 状态码 | 释义
---|---
200 | 获取成功
400 | 请求参数有误,请检查参数
401 | 用户验证有误,请检查验证信息
403 | 禁止访问,有可能超出了请求次数限制
500 | 后端服务错误
