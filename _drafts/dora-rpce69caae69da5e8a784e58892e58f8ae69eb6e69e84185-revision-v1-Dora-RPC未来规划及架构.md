---
id: 223
title: Dora-RPC未来规划及架构
date: 2018-10-10T14:07:17+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/223
permalink: /archives/223
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div>
    理论上这东东不是我一个人能写完的，期望有兴趣的朋友也一起参与下开发
  </div>
  
  <div>
  </div>
  
  <div>
    Dora-RPC旨在制作一套PHP企业级的业务架构，通过这个架构可以快速实现
  </div>
  
  <div>
    <ul>
      <li>
        内部SAAS及完善的监控管理
      </li>
      <li>
        动态可伸缩式的后端
      </li>
      <li>
        更简单的内部API集成管理
      </li>
      <li>
        分布式调试支撑
      </li>
    </ul>
  </div>
  
  <div>
    <span STYLE="line-height: 21px;">具体YY结构如下图：</span>
  </div>
  
  <p>
    <a HREF="http://photo.blog.sina.com.cn/showpic.html#blogid=54ef39890102vs3h&#038;url=http://album.sina.com.cn/pic/001yr0dXzy6VYB5M4kHc3" TARGET="_blank"><img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://s4.sinaimg.cn/mw690/001yr0dXzy6VYB5M4kHc3&690" NAME="image_operate_33671444068048971" ALT="Dora-RPC未来规划及架构" TITLE="Dora-RPC未来规划及架构" /></a>
  </p>
  
  <div>
  </div>
  
  <div>
    Dora-RPC将服务器分为两组：前端和后端。
  </div>
  
  <div>
    <ul>
      <li>
        前端：负责承载服务请求，对后端提供的服务进行拼装。支持同步、异步 单个、多个任务下发。
      </li>
      <li>
        后端：负责提供类似FPM的容器常驻内存接收前端请求。
      </li>
      <li>
        监视服务：负责监视后端工作状态及配置同步
      </li>
      <li>
        日志服务：日志收集及统计，服务预警及日志查询。
      </li>
    </ul>
  </div>
  
  <div>
  </div>
  
  <div>
    项目地址：https://github.com/xcl3721/Dora-RPC
  </div>
  
  <div>
  </div>
  
  <div>
    目前实现情况：只是完成了基础的RPC功能 支持同步、异步 单个、多个任务下发
  </div>
  
  <div>
  </div>
  
  <div>
    缺少功能：
  </div>
  
  <div>
    <ul>
      <li>
        本地日志监控及推送（swoole的process实现）
      </li>
      <li>
        分布式日志统计及收集
      </li>
      <li>
        分布式调试日志及查询
      </li>
      <li>
        日志查询及索引
      </li>
      <li>
        预警通知系统，支持邮件，短信，及自定义模块
      </li>
      <li>
        后端前端服务器监控及界面
      </li>
      <li>
        配置管理及推送机制
      </li>
      <li>
        更好的监控规则
      </li>
      <li>
        容器工作状态监控
      </li>
      <li>
        异步队列
      </li>
    </ul>
  </div>