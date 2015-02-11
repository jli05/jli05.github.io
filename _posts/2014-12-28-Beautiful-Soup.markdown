---
title: Using Beautiful Soup
layout: post
---
When using Beautiful Soup, I found `lxml` is a much more robust parser than the default one. Below is an example to construct the soup.

```python
import requests as req
import bs4

r = req.get(url)
soup = bs4.BeautifulSoup(r.text, 'lxml')
```

