---
id: 264
title: Openfire 集群，周期性挂掉猜测
date: 2018-10-10T14:15:30+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/264
permalink: /archives/264
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  openfire使用的时候常常会出现用一段时间后集群内服务器自动脱离集群服务器组的情况</p> 
  
  <p>
    看了下log<br /><span STYLE="color: rgb(96, 96, 96);">This senior Member(Id=1,Timestamp=2011-07-13 15:25:26.138, Address=192.168.1.100:8088,MachineId=26980, Location=process:32340@xxxx) appears to have beendisconnected from another senior Member(Id=2, Timestamp=2011-07-1216:13:03.482, Address=192.168.1.104:8088, MachineId=26984,Location=process:22755@xxxx); stopping cluster service.</p> 
    
    <p>
      找了一圈论坛发现</span>原解答地址：http://kr.forums.oracle.com/forums/thread.jspa?threadID=575915
    </p>
    
    <p>
      这个帖子建议更改coherence这个插件内的配置<br />coherence-cache-config.xml<br />内的<br />join-timeout-milliseconds改大一些即可
    </p>
    
    <p>
      目前还在测试中，若发现新问题再分享:)
    </p>