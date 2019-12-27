---
title: Hexo
tags: 学习
categories: hexo
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

### 一键提交并发布

``` bash
$ hexo deploy --generate
$ hexo --generate deploy
$ hexo d -g
$ hexo -g d
```

### 修改首页不显示全文

设置 `themes` 文件对应的主题样式下的 `_config.yml` 配置文件

```
auto_excerpt:
  enable: true
  length: 150
```
然后运行命令行

``` bash
$ hexo d -g
```