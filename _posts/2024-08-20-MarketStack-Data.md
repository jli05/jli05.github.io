---
title: MarketStack Data
layout: post
---

The end-of-day Nasdaq data is delayed by about 1h11m, NYSE data is delayed by .

```sh
% curl 'http://api.marketstack.com/v1/eod/latest?access_key=<access_key>&symbols=AAPL'
{"pagination":{"limit":100,"offset":0,"count":1,"total":1},"data":[{"open":225.72,"high":225.99,"low":223.04,"close":225.89,"volume":40639000.0,"adj_high":225.99,"adj_low":223.04,"adj_close":225.89,"adj_open":225.72,"adj_volume":40687813.0,"split_factor":1.0,"dividend":0.0,"symbol":"AAPL","exchange":"XNAS","date":"2024-08-19T00:00:00+0000"}]}

% curl 'http://api.marketstack.com/v1/intraday/latest?access_key=<access_key>&symbols=AAPL&interval=1min'
{"pagination":{"limit":100,"offset":0,"count":1,"total":1},"data":[{"open":226.06,"high":227.185,"low":225.5,"last":226.43,"close":225.89,"volume":495741.0,"date":"2024-08-20T19:59:00+0000","symbol":"AAPL","exchange":"IEXG"}]}
```
