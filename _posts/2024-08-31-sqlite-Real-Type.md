---
title: sqlite text for Real type data
layout: post
---

It seems sqlite treats any text as zero for Real type.

```
sqlite> create table tab(
   ...>   a text,
   ...>   b real
   ...> );
sqlite> insert into tab values ('a', null), ('a', 'null'), ('a', 'sdfsd'), ('b', 5);
sqlite> select * from tab;
a|
a|null
a|sdfsd
b|5.0
sqlite> select b + 3 from tab;                                                  

3
3
8.0
```
