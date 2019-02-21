---
title: 'My CTF Journey: WeChall  Writeup(1)'
date: 2016-07-16 20:49:45
tags: 
- CTF
categories: Hacking 
---
以前想写一些技术类的内容总是无从下手，因为多少会有些枯燥。刚好最近的闲暇时光在做CTF题，都是一些比较有意思的挑战（类似于谜题），决定从这里试水。有兴趣的朋友可以到www.wechall.net试一试。
<!-- more -->

### Prime Factory
>Your task is simple:
Find the first two primes above 1 million, whose separate digit sums are also prime.
As example take 23, which is a prime whose digit sum, 5, is also prime.
The solution is the concatination of the two numbers,
Example: If the first number is 1,234,567
and the second is 8,765,432,
your solution is 12345678765432

这道题比较简单。找到大于一百万且各位数字相加后也为质数的前两个质数，把这两个数串接起来就是答案。

```
#include <stdio.h>
#include <math.h>
int is_prime(int i);
int digitsum(int i);
int main()
{
    int i,x1,x2,count=0;
    for(i=1000001;count<2;i++)
    {
        if(is_prime(i))
            if(is_prime(digitsum(i)))
            {
                printf("%d\n",i);
                count++;
            }
    }

    return 0;
}

int is_prime(int i)
{
    int t;
    for(t=2;t<ceil(sqrt(i))+1;t++)
        if(i%t==0) return 0;

    return 1;
}

int digitsum(int i)
{
    int sum=0,t=10;
    while(i>=1)
    {
        sum+=i%t;
        i=floor(i/10);
    }
    return sum;
}
```

C语言写起来比较复杂，要是用Python的话就会简洁很多：

```
import subprocess as sp

t = 1000000
def isPrime(x):
    t = sp.check_output(("factor %d" % x).split())
    if t.split()[1] == str(x):
        return True
    return False

while True:
    t += 1
    if isPrime(t) and isPrime(sum([int(x) for x in str(t)])):
        print t
```
（这段代码来自[陈文青的主页](http://winkar.github.io/2015/01/24/wechall.html)）
是不是看起来简洁多了？掌握一门脚本语言真的很重要啊。
p.s. 其实还有更简便的：上网搜索质数表，翻到100万左右的数，挨个校验一下很快就能找到答案。某种程度上我认为这是更hacker的方法。

### Training: Get Sourced 
>The solution is hidden in this page
*Use View Sourcecode to get it*

很基础的CTF题。人家都讲了去看源代码，那就在网页上右键->查看源代码，拖动到最后一行得到答案。                                                                                                                                                                                                                                           
>You are looking for this password: html_sourcecode     

### Training: Stegano I
>This is the most basic image stegano I can think of.

然后给了一张视线模糊的图。
Stegano是Steganography的短写，意思是隐写术（[Wiki:Stegenography](https://en.wikipedia.org/wiki/Steganography)）。此处是通过一定方法在图片中隐藏了某些信息，也是常见的题目。第一反应是先查看十六进制数据。不过要是懒的话可以先右键用记事本打开，就能找到答案:
> Look what the hex-edit revealed: passwd:steganoI

### Training: Crypto - Caesar I
>As on most challenge sites, there are some beginner cryptos, and often you get started with the good old caesar cipher.
I welcome you to the WeChall style of these training challenges :)
Enjoy!
接着是下面一段密文：
>BPM YCQKS JZWEV NWF RCUXA WDMZ BPM TIHG LWO WN KIMAIZ IVL GWCZ CVQYCM AWTCBQWV QA JZKMMUKNQAJP

凯撒密码（[Wiki: Caesar cipher](https://en.wikipedia.org/wiki/Caesar_cipher)）是密码学里的入门科目,基本原理就是把字母表顺次移动一定的位数k后重新映射。比如k=3时，密文D就代表A，you加密后就变成了brx.
实在懒得写程序了，网上搜了一个现成的解码器，只不过k的值要手工试。得到答案:
>*我忘了，也懒得再做一遍抱歉...*

当然也有简洁但高深一点的做法，利用Linux系统下的tr命令：
```
echo "VJG SWKEM DTQYP HQZ LWORU QXGT VJG NCBA FQI QH ECGUCT CPF AQWT WPKSWG UQNWVKQP KU UDCJRKRJUROQ" | tr [A-Z] [Y-ZA-X]
```
诺，就是这样。

今天就到这里吧。写下这些的初衷是记录自己的爱好同时也是日常娱乐项目。专业外人士能看到这里真的很不容易，衷心地祝您晚安。

*不爱看也没事儿，反正我还(ke)是(neng)会继续写的。*