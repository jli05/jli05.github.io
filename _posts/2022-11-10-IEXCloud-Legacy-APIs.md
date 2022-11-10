---
title: IEXCloud Legacy API examples
layout: post
---

Although I'd prefer a uniform `/?key=value&key=value` format for the [IEXCloud](https://iexcloud.io) API.

```
https://cloud.iexapis.com/stable/stock/ORA-FP/quote?token=<token>
https://cloud.iexapis.com/stable/ref-data/exchange/BOM/symbols?token=<token>
https://cloud.iexapis.com/stable/stock/ORA-FP/chart/5d?token=<token>
https://cloud.iexapis.com/stable/fx/historical?symbols=EURUSD,GBPUSD&last=3&token=<token>
https://cloud.iexapis.com/stable/stock/market/batch?symbols=AAPL,QQQ&token=<token>&types=previous
```
