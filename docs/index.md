<a href="https://www.ambisafe.co/">![test](img/logo_red.png)</a>
**********

# About EToken History service
Service to store and retrieve transactions for EToken. Currently for **Transfer** and **TransferToICAP** events.  
Service deployed on <https://etoken-history.ambisafe.co>

**********

## Store
To store transaction - **POST /tx** with body:  
1) **Transfer** event must contain **symbol** field
```json
{
  "from":"0x7c60f73fbf669ab9df578c70c25a5c31788720ea",
  "name":"Transfer",
  "reference":"",
  "transactionHash":"0x10b3aa77a76fd6560c60379223dd1a4473d9232efdced08100a0a44f7fcc153f",
  "timestamp":1468517882,
  "value":1,
  "blockNumber":1884807,
  "to":"0xe51e53a8e678caf0e6dbcbf4f76d39cad9453595",
  "confirmations":1,
  "version":1,
  "logIndex":15,
  "symbol":"EXMPL",
  "transactionIndex":12
}
```
2) **TransferToICAP** must contain **icap** field
```json
{
  "from":"0x7c60f73fbf669ab9df578c70c25a5c31788720ea",
  "name":"TransferToICAP",
  "reference":"",
  "transactionHash":"0x10b3aa77a76fd6560c60379223dd1a4473d9232efdced08100a0a44f7fcc153f",
  "timestamp":1468517882,
  "value":1,
  "blockNumber":1884807,
  "to":"0xe51e53a8e678caf0e6dbcbf4f76d39cad9453595",
  "confirmations":1,
  "version":1,
  "logIndex":15,
  "icap":"XE80EXMAMBI0DIMA0HI0",
  "transactionIndex":12
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
      "from":"0x7c60f73fbf669ab9df578c70c25a5c31788720ea",
      "name":"TransferToICAP",
      "reference":"",
      "transactionHash":"0x10b3aa77a76fd6560c60379223dd1a4473d9232efdced08100a0a44f7fcc153f",
      "timestamp":1468517882,
      "value":1,
      "blockNumber":1884807,
      "to":"0xe51e53a8e678caf0e6dbcbf4f76d39cad9453595",
      "confirmations":1,
      "version":1,
      "logIndex":15,
      "icap":"XE80EXMAMBI0DIMA0HI0",
      "transactionIndex":12
    },
    {
      "from":"0x7c60f73fbf669ab9df578c70c25a5c31788720ea",
      "name":"Transfer",
      "reference":"",
      "transactionHash":"0x10b3aa77a76fd6560c60379223dd1a4473d9232efdced08100a0a44f7fcc153f",
      "timestamp":1468517882,
      "value":1,
      "blockNumber":1884807,
      "to":"0xe51e53a8e678caf0e6dbcbf4f76d39cad9453595",
      "confirmations":1,
      "version":1,
      "logIndex":15,
      "symbol":"EXMPL",
      "transactionIndex":12
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
      "from":"0x7c60f73fbf669ab9df578c70c25a5c31788720ea",
      "name":"TransferToICAP",
      "reference":"",
      "transactionHash":"0x10b3aa77a76fd6560c60379223dd1a4473d9232efdced08100a0a44f7fcc153f",
      "timestamp":1468517882,
      "value":1,
      "blockNumber":1884807,
      "to":"0xe51e53a8e678caf0e6dbcbf4f76d39cad9453595",
      "confirmations":1,
      "version":1,
      "logIndex":15,
      "icap":"XE80EXMAMBI0DIMA0HI0",
      "transactionIndex":12
    },
    {...},
    {
      "from":"0x1ff21eca1c3ba96ed53783ab9c92ffbf77862584",
      "name":"TransferToICAP",
      "reference":"",
      "transactionHash":"0x06fd869e881d9015d801a7b01540c57bcb386db0c5c2e2d31b4711a97ac5b9df",
      "timestamp":1465890273,
      "value":111,
      "blockNumber":1884806,
      "to":"0xe51e53a8e678caf0e6dbcbf4f76d39cad9453595",
      "confirmations":1,
      "version":1,
      "logIndex":15,
      "icap":"XE80EXMAMBI0DIMA0HI0",
      "transactionIndex":12
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