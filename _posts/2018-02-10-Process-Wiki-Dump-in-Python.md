---
title: Process Wiki Dump in Python 
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

We could either use `xmllint`, but `xmllint` can only process small abstract-only corpus, as opposed to the full-text "pages-articles" corpus.

[Query XML with namespace](https://stackoverflow.com/questions/8264134/xmllint-failing-to-properly-query-with-xpath?rq=1)

or `lxml.etree.ElementTree`. For security we should really use `defusedxml` or `defusedexpat` but they're not production-ready as of Feb 2018 and have a less rich API.

```python
import bz2 
import lxml.etree

def articles_iter(dump='simplewiki-latest-pages-articles.xml.bz2',
                  keyphrase=None):
    ''' Extract pages with keyphrase '''
    with bz2.open(dump) as pages:
        # Build ElementTree from this file
        tree = lxml.etree.ElementTree(file=pages)

        # {*} matches any namespace 
        # if xmlns is defined in the XML doc
        for node in tree.iter(tag='{*}text'):
            if node.text is None:
                continue
            if keyphrase is None:
                yield node.text
            elif keyphrase in node.text:
                yield node.text
```
