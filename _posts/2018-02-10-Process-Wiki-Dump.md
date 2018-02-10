---
title: Process Wiki Dump 
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

We could either use `xmllint`, but `xmllint` can only process small abstract-only corpus, as opposed to the full-text "pages-articles" corpus.

[Query XML with namespace](https://stackoverflow.com/questions/8264134/xmllint-failing-to-properly-query-with-xpath?rq=1)

or `lxml.etree.ElementTree`. For security we should really use `defusedxml` or `defusedexpat` but they're not production-ready as of Feb 2018 and have a less rich API.

```python
import gzip
import lxml.etree
dumpfile = gzip.open(<dump filename>)
tree = lxml.etree.ElementTree(file=dumpfile)

for node in tree.iter(tag='{*}text'):
    # {*} could match any namespace, which is specified
    # by the xmlns attribute in the XML file if it exists
    # Effectively we iterate through //{ns}page/{ns}text

    # Process node.text
```
