# About EToken History service
Service to store and retrieve transactions for EToken. Currently for **Transfer** and **TransferToICAP** events.  
Service deployed on <https://etoken-history.ambisafe.co>

**********

## Store
To store transaction - **POST /tx** with body:  
1) **Transfer** event must contain **symbol** field
```json
{  
  "from":"0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
  "name":"Transfer",
  "reference":"",
  "transactionHash":"0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
  "timestamp":1465890273,
  "value":111,
  "blockNumber":100,
  "symbol":"EXMPL",
  "to":"0x53786e5722f854a62783395dcdc27d633a9b063e",
  "logIndex":1,
  "transactionIndex":0
}
```
2) **TransferToICAP** must contain **icap** field
```json
{
  "from": "0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
  "name": "TransferToICAP",
  "reference": "",
  "transactionHash": "0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
  "timestamp": 1465890273,
  "value": 111,
  "blockNumber": 100,
  "to": "0x53786e5722f854a62783395dcdc27d633a9b063e",
  "logIndex": 2,
  "icap": "XE80EXMAMBI0DIMA0HI0",
  "transactionIndex": 0
}
```

**********

## Get by recipient
To get transactions for specific recipient - **GET /tx/{recipient}**, where **{recipient}** is the address and can be with/without "0x" and in lower/upper case.
> **NOTICE: in response will be all events.
<br>If you want only specific events, try to use [additional parameters](#parameters)**

Response will be like:
```json
{
  "total": 2,
  "result": [
    {
      "from": "0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
      "name": "TransferToICAP",
      "reference": "",
      "transactionHash": "0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
      "timestamp": 1465890273,
      "value": 111,
      "blockNumber": 104,
      "to": "0x53786e5722f854a62783395dcdc27d633a9b063e",
      "logIndex": 2,
      "icap": "XE80EXMAMBI0DIMA0HI0",
      "transactionIndex": 0
    },
    {
      "from": "0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
      "name": "Transfer",
      "reference": "",
      "transactionHash": "0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
      "timestamp": 1465890273,
      "value": 111,
      "blockNumber": 104,
      "symbol": "EXMPL",
      "to": "0x53786e5722f854a62783395dcdc27d633a9b063e",
      "logIndex": 1,
      "transactionIndex": 0
    }
  ],
  "nextRequest": null
}
```

**********

## Get by ICAP
You can get all transactions by ICAP - **GET /icap/{icap}**.
<br>With this request you can use **max** and **skip** parameters.
<br>Response will be like:
```json
{
  "total": 5,
  "result": [
    {
      "from": "0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
      "name": "TransferToICAP",
      "reference": "",
      "transactionHash": "0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
      "timestamp": 1465890273,
      "value": 111,
      "blockNumber": 100,
      "to": "0x53786e5722f854a62783395dcdc27d633a9b063e",
      "logIndex": 2,
      "icap": "XE80EXMAMBI0DIMA0HI0",
      "transactionIndex": 0
    },
    {...},
    {
      "from": "0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
      "name": "TransferToICAP",
      "reference": "",
      "transactionHash": "0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
      "timestamp": 1465890273,
      "value": 111,
      "blockNumber": 104,
      "to": "0x53786e5722f854a62783395dcdc27d633a9b063e",
      "logIndex": 2,
      "icap": "XE80EXMAMBI0DIMA0HI0",
      "transactionIndex": 0
    }
  ],
  "nextRequest": null
}
```

**********

## Parameters
You can also use additional parameters and combine them:

1. **max** - max entries in response;
2. **skip** - skip several entries;
3. **symbol** - with specific currency symbol;
4. **name** - with specific event name.
 
If you specified **max** parameter and there are more entries in the table, you received url-link to next portion of data with the same size and filters in **nextRequest** field.

Lets execute request with **max=2** parameter:  
**GET /tx/{recipient}?max=2**  
Response will be like:
```json
{
  "total": 2,
  "result": [...],
  "nextRequest": "https://fm93pbrndi.execute-api.eu-central-1.amazonaws.com/stage/tx/0x53786e5722f854a62783395dcdc27d633a9b063e?start=000000104-1&max=2"
}
```