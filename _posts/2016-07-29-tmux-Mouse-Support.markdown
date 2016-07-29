---
title: tmux Mouse Support 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$',i'$'], ['\\(','\\)']]}});</script>

[http://tangledhelix.com/blog/2012/07/16/tmux-and-mouse-mode/](http://tangledhelix.com/blog/2012/07/16/tmux-and-mouse-mode/)

Create `.tmux.conf` under the `$HOME` directory, and type

```bash
set -g mode-mouse on
set -g mouse-resize-pane on
set -g mouse-select-pane on
set -g mouse-select-window on
```
