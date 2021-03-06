---
id: 126
title: '[转载]linux sed命令'
date: 2011-04-18T14:22:16+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/126
permalink: /archives/126
categories:
  - 经典技巧
tags:
  - 转载
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  好东西窃走了！~</p> 
  
  <div class="blogzz_abstract borderc" style="padding-top:15px;margin:20px 0; border:none; border-top:1px dotted #ccc;">
    <div class="blogzz_ainfo" style="margin-bottom:12px;">
      <span style="margin-right:25px;"><strong>原文地址：</strong><a target="_blank" href="http://blog.sina.com.cn/s/blog_6238358c0100hb0z.html" title="linux sed命令">linux sed命令</a></span><span><strong>作者：</strong><a href="http://blog.sina.com.cn/u/1647850892"  title="水境之" target="_blank" >水境之</a></span>
    </div>
    
    <div class="blogzz_acon">
      <p>
        [root@linux ~]# sed [-nefr] [动作]<br />参数&#8758;<br /><font COLOR="#FF7E00">-n &#8758;使用安静(silent)模式。在一般 sed的用法中，所有来自 STDIN<br /> 的资料一般都会被列出到萤幕上。但如果加上 -n 参数后，则只有经过<br /> sed 特殊处理的那一行(或者动作)才会被列出来。<br />-e &#8758;直接在指令列模式上进行 sed 的动作编辑；<br />-f &#8758;直接将 sed 的动作写在一个档案内， -f filename 则可以执行filename 内的<br /> sed 动作；<br />-r &#8758;sed 的动作支援的是延伸型正规表示法的语法。(预设是基础正规表示法语法)<br />-i &#8758;直接修改读取的档案内容，而不是由萤幕输出。</font>
      </p>
      
      <p>
        <font COLOR="#FF7E00">动作说明&#8758; [n1[,n2]]function<br />n1, n2 &#8758;不见得会存在，一般代表『选择进行动作的行数』，举例来说，如果我的动作<br /> 是需要在 10 到 20 行之间进行的，则『 10,20[动作行为] 』</font>
      </p>
      
      <p>
        <font COLOR="#FF7E00">function 有底下这些咚咚&#8758;<br />a &#8758;新增， a的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～<br />c &#8758;取代， c 的后面可以接字串，这些字串可以取代n1,n2 之间的行！<br />d &#8758;删除，因为是删除啊，所以 d后面通常不接任何咚咚；<br />i &#8758;插入， i的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；<br />p &#8758;列印，亦即将某个选择的资料印出。通常 p 会与参数sed -n 一起运作～<br />s &#8758;取代，可以直接进行取代的工作哩！通常这个 s的动作可以搭配<br /> 正规表示法！例如 1,20s/old/new/g 就是啦！</font><br />范例&#8758;
      </p>
      
      <p>
        范例一&#8758;将 /etc/passwd 的内容列出，并且我需要列印行号，同时，请将第 2~5 行删除！<br />[root@linux ~]# nl /etc/passwd | sed &#8216;2,5d&#8217;<br /> 1 root:x:0:0:root:/root:/bin/bash<br /> 6 sync:x:5:0:sync:/sbin:/bin/sync<br /> 7 shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown<br />&#8230;..(后面省略)&#8230;..<br /><font COLOR="#FF7E00"># 看到了吧？因为 2-5 行给他删除了，所以显示的资料中，就没有 2-5行棉～<br /># 另外，注意一下，原本应该是要下达 sed -e 才对，没有 -e 也行啦！<br /># 同时也要注意的是， sed 后面接的动作，请务必以 &#8221; 两个单引号括住喔！<br /># 而，如果只要删除第 2 行，可以使用 nl /etc/passwd | sed &#8216;2d&#8217; 来达成，<br /># 至于第 3 到最后一行，则是 nl /etc/passwd | sed &#8216;3,$d&#8217; 的啦！</font>
      </p>
      
      <p>
        范例二&#8758;承上题，在第二行后(亦即是加在第三行)加上『drink tea?』字样！<br />[root@linux ~]# nl /etc/passwd | sed &#8216;2a drink tea&#8217;<br /> 1 root:x:0:0:root:/root:/bin/bash<br /> 2 bin:x:1:1:bin:/bin:/sbin/nologin<br />drink tea<br /> 3 daemon:x:2:2:daemon:/sbin:/sbin/nologin<br /><font COLOR="#FF7E00"># 嘿嘿！在 a后面加上的字串就已将出现在第二行后面棉！那如果是要在第二行前呢？<br /># nl /etc/passwd | sed &#8216;2i drink tea&#8217; 就对啦！</font>
      </p>
      
      <p>
        范例三&#8758;在第二行后面加入两行字，例如『Drink tea or &#8230;..』『drink beer?』<br />[root@linux ~]# nl /etc/passwd | sed &#8216;2a Drink tea or &#8230;&#8230;<br />> drink beer ?&#8217;<br /> 1 root:x:0:0:root:/root:/bin/bash<br /> 2 bin:x:1:1:bin:/bin:/sbin/nologin<br />Drink tea or &#8230;&#8230;<br />drink beer ?<br /> 3 daemon:x:2:2:daemon:/sbin:/sbin/nologin<br /><font COLOR="#FF7E00"># 这个范例的重点是，我们可以新增不只一行喔！可以新增好几行～<br /># 但是每一行之间都必须要以反斜线 来进行新行的增加喔！所以，上面的例子中，<br /># 我们可以发现在第一行的最后面就有 存在啦！那是一定要的喔！</font>
      </p>
      
      <p>
        范例四&#8758;我想将第2-5行的内容取代成为『No 2-5 number』呢？<br />[root@linux ~]# nl /etc/passwd | sed &#8216;2,5c No 2-5 number&#8217;<br /> 1 root:x:0:0:root:/root:/bin/bash<br />No 2-5 number<br /> 6 sync:x:5:0:sync:/sbin:/bin/sync<br /><font COLOR="#FF7E00"># 没有了 2-5 行，嘿嘿嘿嘿！我们要的资料就出现啦！</font>
      </p>
      
      <p>
        范例五&#8758;仅列出第 5-7 行<br />[root@linux ~]# nl /etc/passwd | sed -n &#8216;5,7p&#8217;<br /> 5 lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin<br /> 6 sync:x:5:0:sync:/sbin:/bin/sync<br /> 7 shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown<br /><font COLOR="#FF7E00"># 为什么要加 -n 的参数呢？您可以自行下达 sed &#8216;5,7p&#8217;就知道了！(5-7行会重复输出)<br /># 有没有加上 -n 的参数时，输出的资料可是差很多的喔！</font>
      </p>
      
      <p>
        范例六&#8758;我们可以使用 ifconfig 来列出 IP ，若仅要 eth0 的 IP 时？<br />[root@linux ~]# ifconfig eth0<br />eth0 Link encap:Ethernet HWaddr00:51:FD:52:9A:CA<br /> inet addr:192.168.1.12 Bcast:192.168.1.255 Mask:255.255.255.0<br /> inet6 addr: fe80::250:fcff:fe22:9acb/64 Scope:Link<br /> UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1<br />&#8230;..(以下省略)&#8230;..<br /><font COLOR="#FF7E00"># 其实，我们要的只是那个 inet addr:..那一行而已，所以棉，利用 grep 与sed 来捉<br /></font>[root@linux ~]# ifconfig eth0 | grep &#8216;inet &#8216;|sed&#8217;s/^.*addr://g&#8217;|sed &#8216;s/Bcast.*$//g&#8217;<br />[root@linux ~]# ifconfig eth0 | grep &#8216;inet &#8216;|sed&#8217;s/^.*addr://g&#8217;|sed &#8216;s/ Bcast.*$//g&#8217;<br /><font COLOR="#FF7E00">#这两行命令差不多，但是第二行在Bacst前有个空格，一定要注意，说明连空格在内都去除了，可以使用<br />#上面两行命令重向到两个文本文件中，例如1.txt 2.txt 仔细查看这两个文件的大小<br /># 您可以将每个管线 (|) 的过程都分开来执行，就会晓得原因棉！<br /># 去头去尾之后，就会得到我们所需要的 IP 亦即是 192.168.1.12 棉～</font>
      </p>
      
      <p>
        范例七&#8758;将 /etc/man.config 档案的内容中，有 MAN 的设定就取出来，但不要说明内容。<br />[root@linux ~]# cat /etc/man.config | grep &#8216;MAN&#8217;| sed &#8216;s/#.*$//g&#8217;|sed &#8216;/^$/d&#8217;<br /><font COLOR="#FF7E00"># 每一行当中，若有 # 表示该行为注解，但是要注意的是，有时候，<br /># 注解并不是写在第一个字元，亦即是写在某个指令后方，如底下的模样&#8758;<br /># 『shutdown -h now # 这个是关机的指令』，注解 # 就在指令的后方了。<br /># 因此，我们才会使用到将 #.*$ 这个正规表示法！</font>
      </p>
      
      <p>
        范例八&#8758;利用 sed 直接在 ~/.bashrc 最后一行加入『# This is a test』<br />[root@linux ~]# sed -i &#8216;$a # This is a test&#8217; ~/.bashrc<br /><font COLOR="#FF7E00"># 上头的 -i 参数可以让你的 sed直接去修改后面接的档案内容喔！而不是由萤幕输出。<br /># 至于那个 $a 则代表最后一行才新增的意思。</font>
      </p></p>
    </div>
  </div>