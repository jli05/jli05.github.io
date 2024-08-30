---
title: tsterm intraday FX turned off
layout: post
---

```sqlite
update info set value = 'R' where name in ('GBPUSD', 'EURUSD', 'USDCAD', 'USDJPY', 'USDCHF', 'USDTWD', 'USDHKD', 'AUDUSD', 'NZDUSD', 'USDCNH', 'USDNOK') and key == 'lifecycle';
select * from info where name like '%USD%' and key == 'lifecycle';
select * from info where name like '%USD%' and key == 'lifecycle' and value == 'A';
```
