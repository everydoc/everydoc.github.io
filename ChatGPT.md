# 部署了一个ChatGPT

ChatGPT火爆了一段时间了，但是基于一些不可描述的原因，不能注册使用。为了方便大家体验，我部署了一个服务，能简单地体验一下ChatGPT的强大。话不多说，直接展示几个例子截图。文章最后放上体验地址和体验方式。

 ## 探索问题



![](/Users/alan/Library/Application Support/typora-user-images/image-20230327203719150.png)

这里我探索了一个技术问题，但是实际上你也可以问很多其他方面的问题，比如`为什么行人靠右`、`世界上有多少个国家`、`为什么饭吃多了肚子痛`。。。

重点是你可以一直追问，就像是对话一样。如果这都不能惊艳到你，那我们接下来让它给我们写代码吧！

 ## 写代码

![image-20230327203801977](/Users/alan/Library/Application Support/typora-user-images/image-20230327203801977.png)

 如果你不满意，还可以让它改改，当返回的内容达到最大限度时，只要发送**继续** 就能接着输出未输出完成内容。

![image-20230327203851345](/Users/alan/Library/Application Support/typora-user-images/image-20230327203851345.png)

不用怀疑，复制出这个页面就能用，不信你看！！！

<img src="/Users/alan/Desktop/截屏2023-03-27 20.40.06.png" alt="截屏2023-03-27 20.40.06" style="zoom:50%;" />

<img src="/Users/alan/Desktop/截屏2023-03-27 20.40.31.png" alt="截屏2023-03-27 20.40.31" style="zoom:25%;" />

还不够惊艳吗？那咱们来写一部小说吧！

 ## 写小说

![image-20230327204335036](/Users/alan/Library/Application Support/typora-user-images/image-20230327204335036.png)

![image-20230327204355349](/Users/alan/Library/Application Support/typora-user-images/image-20230327204355349.png)

![image-20230327204429307](/Users/alan/Library/Application Support/typora-user-images/image-20230327204429307.png)



## 结语

怎么样？是不是感觉自己的工作保不住了？？？

重点来了，提供一个体验地址：https://chatter.hk.imjcker.com

发送关键字：**chatter** 到我的公众号：**天府书虫**，获取访问密钥！！！

![image-20230327205448879](/Users/alan/Library/Application Support/typora-user-images/image-20230327205448879.png)

另外：

1. 因为带宽不高的原因，网速慢请见谅！！！
2. 请不要探索敏感话题，你我都懂！！！










白嫖ChatGPT



今天在这里，给大家一个白嫖ChatGPT的方法，方法简便快捷，小白都能操作。

另外：如果有不会的，可以后台联系我或者留言咨询。或者直接使用本人公众号菜单提供的ChatGPT服务。

不说废话，直接上干货。



前提条件：

1. GitHub账号
2. docker



首先，GitHub账号是用来获取免费的ChatGPT秘钥的，所以这个是必要的条件。



如果你是前端开发者，那么你可以不用docker，docker主要是为了方便部署使用；你也可以通过其他方式部署，比如NGINX。



首先是申请免费秘钥：https://github.com/chatanywhere/GPT_API_free

![截屏2023-08-08 01.14.11](/Users/alan/Library/Application Support/typora-user-images/截屏2023-08-08 01.14.11.png)

进入到页面后，点击蓝色字体“领取免费key”，然后将自己获得的免费key复制保存下来。接下来进入第二部，部署ChatGPT服务。



这里使用docker来部署服务，主要是方便快捷，前端同学可以自己在这个地址下载项目，自己修改页面成自己想要的样子：

https://github.com/Chanzhaoyu/chatgpt-web



下面开始部署。

这里假设你已经会使用docker了。



直接执行这个命令就可以了。

```shell
docker run -d --name chatgpt --network host --restart always \
	-e OPENAI_API_KEY=your_api_key \
	-e OPENAI_API_BASE_URL=https://api.chatanywhere.com.cn \
	chenzhaoyu94/chatgpt-web
```

注意：上面代码中的“your_api_key"要换成你在第一部申请到的ChatGPT秘钥。

替换key后，复制整个命令到控制台，直接回车运行，等待启动成功即可。

然后在浏览器打开：http://localhost:3002 即可使用ChatGPT了。

如果你想给你的ChatGPT加个密码，可以使用下面的命令。

```shell
docker run -d --name chatgpt --network host --restart always \
	-e OPENAI_API_KEY=your_api_key \
	-e OPENAI_API_BASE_URL=https://api.chatanywhere.com.cn \
	-e AUTH_SECRET_KEY=your_password \
	chenzhaoyu94/chatgpt-web
```

可以看出来，只是追加了一行代码，将`your_password`替换成你的密码就行了。



![截屏2023-08-08 01.10.44](/Users/alan/Library/Application Support/typora-user-images/截屏2023-08-08 01.10.44.png)



## 结语

感谢开源社区的贡献，用户需注意，因为是免费的key，所以这个key是有流量限制的，当前是 60次/小时，个人使用，完全够用了。

