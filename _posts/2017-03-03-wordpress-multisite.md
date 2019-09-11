---
layout: post
title: 如何用WordPress搭建多站点博客
---
去年购买了域名，创建了英文站点，最近有一些想法很希望写到中文的博客中，虽然之前已经有了CSDN和网易LOFTER，想想既然有自己的域名干脆就在这边建立一个二级域名吧。

这次设立二级域名主要是为了分离不同的语言，建立多语言站点有两种做法：

1、希望内容为中英文两个版本对照出现，适用于机构、商业性站点，需要保持信息的即时、同步，可以求助于WordPress强大的插件库：

    WPGlobus， Polylang， qTranslate 或 xili-language 都是可在单一 WordPress 网站副本中安装的插件。对于多站点 WordPress （即每个语言一个站点），你需要尝试 Multisite Language Switcher， Zanto 或 Multilingual Press， 也可以选择购买 WPML. ——摘自WP支持页面

不过我这次并没有这种需求，所以选择了第二个方案：
2、利用WordPress3.0以后出现的Site Network功能，修改根目录下的wp-config.php和.htacess，创建一个超级管理员身份，就可以创建二级域名（subdomain）了。

根据网上的攻略，可以在public_html根目录下直接添加一个cn文件夹，然后将wordpress相关的文件（除wp-config.php以外）复制到cn目录下，之后就可以访问baixuanwang.com/cn进行配置，生成wp-config.php，如图：

 

但这种方案要求我拥有数据库用户名和密码，尴尬的我当初购买域名时使用的是ReclaimHosting的服务，所以第一个数据库直接扔给他们自动配置了，一时之间想不起来密码，只能作罢。

于是我开始研究官方说法里的multisite，首先第一步备份，第二步让服务器支持wildcard subdomain，否则这边就算配置好了也无法解析。

子域名必须和“通配符子域名”搭配工作，配置需要两步：

    Apache 必须设置为支持通配符。
        Open up the httpd.conf file or the include file containing the VHOST entry for your web account.
        添加此行：

        ServerAlias *.example.com

    在DNS解析中添加一行带有通配符的子域名解析记录：

    A *.example.com

如果你的服务器使用CPanel管理面板，用*.baixuanwang.com新建一个子域名，给wordpress创建的子域名解析权限，不要在CPanel里直接把想要的名字创建了，会造成冲突。

如果这一步无法操作，就需要联系服务器提供商，比如我就给ReclaimHosting发了封邮件表示要申请wildcard subdomain，co-founder Tim Owens很快就回复解答了这个问题，帮我加了wp所需的wildcard配置。

做好准备工作后，首先在public_html/wp-config.php的/* That’s all, stop editing! Happy blogging. */一行之前插入：

define('WP_ALLOW_MULTISITE', true);

刷新站点，重新登录，进入管理面板-Tool-Network Setup，输入你的站点名称和邮箱，该邮箱即为超级管理员。

保存设置后，wp将会提示你两步操作：

A. 更改public_html/wp-config.php，插入上图选中部分到/* That’s all, stop editing! Happy blogging. */一行之前

B. 打开public_html/.htacess（隐藏文件，如果找不到的话，试试更改文件夹权限）

备份文件后，将全部的内容替换为红色箭头所指的文本

完工！

之后就可以用一个账号管理多个站点，两边写的内容也是互不影响的。