---
id: 203
title: log文件owener自动变化问题
date: 2018-10-10T14:01:45+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/203
permalink: /archives/203
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  生产环境封闭，没有办法做bug还原及调试，所以写了一个php错误日志类<br />但是每隔一段时间就会发现log文件由于权限被拒绝导致出错。<br />晓凯说了句看看运行权限，一看log文件已经都被指派给root了，而且时间也很规律，突然想起来是crontab也有定期执行脚本，此时脚本也会写log，但是crontab运行用户是root而不是php所支持的php账户<br />最后找到原因用如下解决<br />crontab -e -u xxxxphpaccount</p>