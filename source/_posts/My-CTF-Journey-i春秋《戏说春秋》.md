---
title: 'My CTF Journey:i春秋《戏说春秋》'
date: 2016-12-05 20:10:53
tags: 
- CTF
categories: Hacking
---
## 第一关 图穷匕见
<!-- more -->
题目给了一张图
![Alt text](http://static2.ichunqiu.com/icq/resources//fileupload/hackgame/XSCQ/wenzi/3.jpg)
string 打开最后有一个字符串：
```
%e6%b2%a1%e9%94%99%e4%bd%a0%e7%9a%84%e6%89%93%e5%bc%80%e6%96%b9%e5%bc%8f%e7%9a%84%e6%ad%a3%e7%a1%ae%e7%9a%84%ef%bc%8c%e9%80%9a%e5%be%80%e4%b8%8b%e4%b8%80%e5%85%b3%e7%9a%84key%e6%98%af%ef%bc%9a%e8%8a%9d%e9%ba%bb%e5%bc%80%e9%97%a8
```
使用url编码解码器得到：
```
没错你的打开方式的正确的，通往下一关的key是：芝麻开门
```

## 第二关 纸上谈兵
一开始什么也没有。查看网页源代码，找到
```html
<input type="hidden" value="W1ZbTDEYCh0cDRkcGw" name="coursescoreid" id="coursescoreid">
```
把value值base64解码后提交，不对。
无奈上网搜了一下，才发现原来在页面里有一行非常长，被挡住了：
```
通关秘钥是一个贝丝第64代的人设计的，你能解开它吗？5be05ouJ5be05ouJ5bCP6a2U5LuZ
```
解码后得到答案：
```
巴拉巴拉小魔仙
```
服了...以后看代码还是要仔细。

## 第三关 窃符救赵
给了一张图：
![Alt text](http://static2.ichunqiu.com/icq/resources//fileupload/hackgame/XSCQ/wenzi/7.jpg)
binwalk之，发现zip的痕迹。重命名为7.zip,解压后得到dh.jpg.
然后就不知道该怎么办了，strings无迹象，stegdetect也没有结果。Key在哪里？
无奈百度之，给这脑洞跪了：原来是要把图片放到百度识图里，将搜索得到的“杜虎符”三个字作为Key提交...

## 第四关 老马识途
![Alt text](http://static2.ichunqiu.com/icq/resources//fileupload/hackgame/XSCQ/wenzi/ls.jpg)
猪圈密码。
![Alt text](http://e.hiphotos.baidu.com/baike/w%3D268/sign=46c6982108d79123e0e0937295345917/91ef76c6a7efce1b115000e3ab51f3deb48f65a4.jpg)
得到HORSE.坑爹的是还要翻译成“马”。

## 第五关 东施效颦
```
趁着西施跟随父母回乡下走亲戚时，东施冒充西施与范蠡对唱情歌，

“啊哈啊啊，哈哈哈哈哈，啊啊啊哈，啊”

但是范蠡根本就不上当，毫不留情的指责东施说，你只知道冒充唱歌，但是你跟不懂西施在歌里对我说的话。那么问题来了，西施的歌里到底藏着什么秘密呢。 
```
很显然这是一段Morse Code，“哈”为长，“啊”为短，四个字母为LOVE。需要注意的是“哈哈哈哈哈”似乎有误，应当是“哈哈哈”。

## 第六关 大义灭亲
讲的是石碏(que)大义灭亲的故事。
![Alt text](http://pic3.zhimg.com/v2-05df26b1b043e1c85671625dd92bfca6_r.jpg)
又到了喜闻乐见的猜密码环节。
我猜不出来。我百度了。密码是
```
shique719
```
719是卫桓公被弑的年份...U fking kidding me?

## 第七关 三令五申
```
“这个盒子里面的是盛放军令状，平时将军写好军令就直接放在里面，我和其他副将都能打开这个盒子查看，然后执行里面的命令。而其他人无法解开这个盒子查看里面的军令，如果大王能够解开这个盒子，在里面放入一张饶恕两位队长的军令，您的爱姬自然能够获救。”
```
要获得开启盒子的密钥。也是没想出来，居然是权限值！
将军是文件的所有者，具有读取、写入和执行权限，副将没有写入权限，只有读取和执行，其他人无权限，因此计算出的权限值为750.这个套路实在是深。
