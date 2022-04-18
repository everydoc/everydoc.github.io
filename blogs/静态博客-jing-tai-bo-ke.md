---
title: 静态博客
date: 2022-01-19 01:31:37.376
updated: 2022-01-19 01:31:37.376
url: https://halo.imjcker.com/archives/jing-tai-bo-ke
categories: 极客工具
tags: 博客 | blog
---



本文主要记录了本站的开发记录,本站是有个免费的搭建在Github的个人静态博客,基于 jekyll 4.0 版本, UI采用bootstrap 4.0.0.  
![jekyll](https://imjcker.com:1990/upload/2022/01/jekyll-264a9f509230481fb7c8e548cc847d61.png)
如果你喜欢, 可以直接[fork](https://github.com/imjcker/imjcker.github.io)使用.  


![site](/assets/2020/blog.imjcker.com.index-1.png)

![site](/assets/2020/blog.imjcker.com.index-2.png)



![site](/assets/2020/blog.imjcker.com.post-1.png)

## GitHub pages

如果你不喜欢使用imjcker.github.io作为你的域名，请继续往下看。

这里以cloudflare为例设置自定义域名

### 一级域名设置

假如你的域名交imjcker.com

你需要在dns解析面板，修改/添加A记录如下。

| Type | Name        | Content        |
| ---- | ----------- | -------------- |
| A    | imjcker.com | 192.30.252.153 |
| A    | imjcker.com | 192.30.252.154 |

然后在项目目录下创建一个名为CNAME的文件

内容你的域名即：imjcker.com

提交到GitHub。

过几分钟打开浏览器访问你的域名你会看到惊喜。



### 二级域名设置

当然，很多朋友的一级域名都拿来做多别的内容，那么现在你就可以使用二级域名作为你的博客网站如本站：

blog.imjcker.com

同样是以cloudflare为例

| Type  | Name | Content           |
| ----- | ---- | ----------------- |
| CNAME | blog | imjcker.github.io |

然后在项目目录下创建一个名为CNAME的文件

内容你的域名即：blog.imjcker.com

提交到GitHub。

过几分钟打开浏览器访问你的域名你会看到惊喜。



## docsify博客

### dockerfile

```dockerfile
FROM node:14.15.4-alpine3.12
MAINTAINER Alan Turing "570577029@qq.com"
LABEL description="构建专属的文档"
WORKDIR /docs
RUN npm install -g docsify-cli
EXPOSE 3000/tcp
RUN docsify init .
ENTRYPOINT docsify serve .


```

### usage

```shell
docker run -d --name docs -p 3000:3000 -v $(pwd):/docs imjcker/docsify:latest
```

[http://localhost:3000](http://localhost:3000) here is your docs

