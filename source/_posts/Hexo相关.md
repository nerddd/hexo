---
title: Hexo相关
mathjax: true
date: 2017-06-02 16:05:45
categories: Hexo
tags: [Hexo]
description: Hexo相关的配置说明
---

# 问题

## Hexo无法正常显示公式

善用主题(theme)，以我使用的next主题为例，打开/themes/next/_config.yml文件，更改mathjax开关为：

```
# MathJax Support
mathjax:
  enable: true
  per_page: true
```

另外，还要在文章(.md文件)头设置开关，只用在有用公式显示的页面才加载Mathjax渲染，不影响其他的页面渲染速度，如下：

```
---
title: index.html
date: 2016-12-28 21:01:30
tags:
mathjax: true
--
```

题外话，可以在/scaffolds/post.md文件中添加mathjax一行，这样每次layout如果是由默认的post 生成新的文章的开头都会有mathjax，可以自己选择true或是false(注意mathjax冒号后面不要掉了空格)，如下：

```
title: {{ title }}
date: {{ date }}
categories: 
tags:
description: 
mathjax: 
```



# 优化

[Hexo的版本控制与持续集成](https://formulahendry.github.io/2016/12/04/hexo-ci/)

