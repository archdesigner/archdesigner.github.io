---
id: 281
title: PHP微服务使用Redis实现事务协调
date: 2018-11-27T11:12:52+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/281
permalink: /archives/281
---
微服务如何简单的实现一致性事务，使用redis，多个进程一起刷redis,隔一秒获取一次某个list，如果list内的数据个数达到预期个数那么就一起提交完成了。

成功：如一个事情六个步骤，list内出现不同的key六个，那么就是完成了。

失败：如果其中有一个fail字符的key那么就是有人失败了，回滚

二次提交确认：如果第一步六个都成了，那么执行一起提交，提交后再获取一次出现fail就代表有接口提交失败了，一起回滚。

超时：如果某个步骤从开始到结束超过五秒就一起回滚

失效:list设置一个ttl，至于怎么通知多个进程一起并行工作multi curl哦