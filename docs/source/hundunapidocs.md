---
title: API Reference

search: true

code_clipboard: true

meta:

- name: description
  content: Documentation for the hundunchain API

---

# 接入说明

## 接入 URL

请使用以下域名发出API请求:

* [http://www.mversebc.com](http://www.mversebc.com/)

## 请求格式

POST请求请在 header 中设置 Content-Type: application/json

## 签名

待定

# 上链API

## 生成地址

* **POST** `/v1/chain/account`

### 响应参数

| **字段**  | **数据类型** | **说明**  |
|---------|----------|---------|
| address | string   | 生成的账户地址 |

### 请求示例

```
curl -X POST 'http://{url}/v1/chain/account' \
-H 'Content-Type: application/json'
```

### 响应示例

```json
{
  "code": 0,
  "message": "请求成功",
  "result": {
    "address": "0xf9574d585818466c1c17bfec55ad0f2dee3707c8"
  }
}
```

## 数据上链

* **POST** `/v1/chain/upchain`

### 请求参数

| **参数**  | **数据类型** | **是否必填** | **说明** |
|---------|----------|----------|--------|
| value   | string   | 是        | 上链内容   |
| address | string   | 否        | 上链地址   |

### 响应参数

| **字段**     | **数据类型** | **说明**   |
|------------|----------|----------|
| index_hash | string   | 上链任务hash |

### 请求示例

```
curl -X POST 'http://{url}/v1/chain/upchain' \
-H 'Content-Type: application/json' \
--data-raw '{"value":"something to upchain","address":"0xf9574d585818466c1c17bfec55ad0f2dee3707c8"}'
```

### 响应示例

```json
{
  "code": 0,
  "message": "请求成功",
  "result": {
    "index_hash": "C0A800920001000000000dba50100014"
  }
}
```

## 验证交易

> Response:

* **GET** `/v1/chain/validate/:index_hash`

### Path参数

| **参数**     | **数据类型** | **是否必填** | **说明**   |
|------------|----------|----------|----------|
| index_hash | string   | 是        | 上链任务hash |

### 响应参数

| **字段**  | **数据类型** | **说明** |
|---------|------|--------|
| msg_id  | string | 消息id   |
| status  | int  | 上链状态，0:上链失败;1:上链成功;2:等待上链;3:队列失败;4:即将执行上链  |
| tx_hash | string | 交易hash |

### 请求示例

```
curl -X GET 'http://{url}/v1/chain/validate/C0A800920001000000000dba50100014'
```

### 响应示例

```json
{
    "code": 0,
    "message": "请求成功",
    "result": {
        "msg_id": "C0A800920001000000000dba50100014",
        "status": 1,
        "tx_hash": "0x551a3af1c345042d2d8f329c1f1bbe51a2d2c92369d2844aceacc0552ac2dd4e"
    }
}
```

## 批量验证交易

* **POST** `/v1/chain/validateBatch`

### 请求参数

| **参数**     | **数据类型** | **是否必填** | **说明** |
|------------|----------|----------|--|
| index_hash | array    | 是        | 上链任务hash的数组 |

### 响应参数

| **字段** | **数据类型** | **说明** |
|----|----------|--------|
| [] | array    | 数组     |
| msg_id | string   | 消息id   |
| status | int      | 上链状态，0:失败 1:成功 2:等待 3:已提交 4:消费失败  |
| tx_hash | string   | 交易hash |

### 请求示例

```
curl -X POST 'http://{url}/v1/chain/validateBatch' \
-H 'Content-Type: application/json' \
--data-raw '{"index_hash":["C0A800920001000000000dba50100014"]}'
```

### 响应示例

```json
{
    "code": 0,
    "message": "请求成功",
    "result": [
        {
            "msg_id": "C0A800920001000000000dba50100014",
            "status": 1,
            "tx_hash": "0x551a3af1c345042d2d8f329c1f1bbe51a2d2c92369d2844aceacc0552ac2dd4e"
        }
    ]
}
```

## 交易详情

* **GET** `/v1/chain/transaction/:tx_hash`

### Path参数

| **参数**     | **数据类型** | **是否必填** | **说明** |
|------------|----------|----------|--|
| tx_hash | string    | 是        | 交易hash |

### 响应参数

| **字段** | **数据类型** | **说明** |
|--|----------|--------|
| id | int    | id     |
| transaction_index | string   | 交易索引   |
| tx_hash | int  | 交易hash |
| block_number | string   | 区块编号 |
| block_hash | string   | 区块hash |
| timestamp | string   | 时间戳 |
| from | string   | 发送地址 |
| to | string   | 接收地址 |
| value | string   | 金额 |
| nonce | string   | 随机数 |
| gas_price | string   | gas价格 |
| gas | string   | gas限额 |
| input | string   | 携带数据 |
| v | string   | 签名v |
| r | string   | 签名r |
| s | string   | 签名s |
| status | int   | 交易状态，0 失败，1 成功 |

# 错误码

| **错误码** | **说明**    | 
|---------|-----------| 
| 0       | 请求成功      | 
| 400     | 参数错误      |  
| 404     | 未知错误      |  
| 405     | 未查询到数据    |  
| 500     | 请求失败      |   
| 5000    | 获取账户地址失败      |   
| 5001    | 获取管理员失败      |     
| 5002    | 获取白名单失败    |                  
| 5003    | 获取节点数量失败  |                         
| 5004    | 获取节点信息失败     | 
| 5005    | 交易发送失败   |                         
| 5006    | 获取交易确认信息失败    |                       
| 5007    | nonce 获取失败 |                       
| 5008    | 获取区块失败 |                     
| 5009    | 未查询交易信息 |                       
| 5010    | 未查询区块信息   |                     
| 5011    | 计算出块节点地址出错   |                  
| 5012    | graphql获取区块出错   |               
| 5013    | graphql获取交易出错   |                     
| 5014    | 链节点连接失败   |