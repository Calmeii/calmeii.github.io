---
layout: post
title: mac下vim的16种配色方案（代码高亮）展示，及配置
tags: [mac, vim]
categories: [mac, vim]
published: true
---

[上一次提到如何使用mac自带的vim写代码](http://www.calmeii.com/mac/vim/mac-vim/),但是
如果没有代码高亮的话，用vim写代码确实不爽，于是，笔者今天花了一个上午的时间把mac下vim
的所有配色试了个遍，

### 下面给大家展示一下mac下所有vim的配色方案的样式。

1. darkblue
![darkblue样式展示]({{site.url}}/images/11-07-15/darkblue.png)

2. delek
![delek样式展示]({{site.url}}/images/11-07-15/delek.png)

3. elflord
![elflord样式展示]({{site.url}}/images/11-07-15/elflord.png)

4. koehler
![koehler样式展示]({{site.url}}/images/11-07-15/koehler.png)

5. murphy
![murphy样式展示]({{site.url}}/images/11-07-15/murphy.png)

6. peachpuff
![peachpuff样式展示]({{site.url}}/images/11-07-15/peachpuff.png)

7. shine
![shine样式展示]({{site.url}}/images/11-07-15/shine.png)

8. torte
![torte样式展示]({{site.url}}/images/11-07-15/torte.png)

9. default
![default样式展示]({{site.url}}/images/11-07-15/default.png)

10. desert
![desert样式展示]({{site.url}}/images/11-07-15/desert.png)

11. evening
![evening样式展示]({{site.url}}/images/11-07-15/evening.png)

12. morning
![morning样式展示]({{site.url}}/images/11-07-15/morning.png)

13. pablo
![pablo样式展示]({{site.url}}/images/11-07-15/pablo.png)

14. ron
![ron样式展示]({{site.url}}/images/11-07-15/ron.png)

15. slate
![slate样式展示]({{site.url}}/images/11-07-15/slate.png)

16. zellner
![zellner样式展示]({{site.url}}/images/11-07-15/zellner.png)

### 下面讲解如何设置这些好看的配色
首先：在终端输入

> $ ls /usr/share/vim/vim73/colors

查看是否有上面提到的某些配色，所有配色均是以.vim结束的，如果有的话，再输入：

> $ cd ~/

到用户主目录，然后输入

> $ vim .vimrc

创建配置文件，将vim的内容设置如下：

>  set nu
>  colorscheme desert
>  syntax on

即配置好desert.vim这种主题方案了，如果想使用其他主题方案，就把desert换成对应的名字就ok啦～～～
下面开始愉快的使用vim编程吧！！！
