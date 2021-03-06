---
id: 128
title: 转：用Imagick替代php的GD库-强大~
date: 2011-05-18T10:45:30+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/128
permalink: /archives/128
categories:
  - 经典技巧
tags:
  - it
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  <p STYLE="TexT-inDenT: 2em;">
    一般用php处理图片都是使用GD库或者GD2的函数库，一般编译php环境都会搭上GD库，大多数开源程序也是用GD来处理图片的，但是它只能现实诸如调整大小、增加水印等基础功能，要想用GD来做复杂图形是非常困难的。
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    还好有个基于命令行的图像处理软件ImageMagick，能实现非常丰富的功能。如果服务器上安装了ImageMagick，php脚本可以使用shell命令来完成，也可以用php的原生函数库Imagick或者MagickWandForPHP函数来调用ImageMagick软件来实现。
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    因为Linux系统下PHP往往没有执行shell的权限，直接用shell来操作ImageMagick不太可能，综合考虑，Imagick函数库连接到ImageMagick软件比较好，而且是面向对象方式的。
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    Linux系统下，编译安装ImageMagick软件
  </p>
  
  <pre>wget ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-6.5.2-7.tar.gztar -zxvf ImageMagick-6.5.2-7.tar.gzcd ImageMagick-6.5.2-7/./configuremakemake install#译PHP原生库Imagickwget http://pecl.php.net/get/imagick-2.2.2.tgztar zxvf imagick-2.2.2.tgzcd imagick-2.2.2//usr/local/webserver/php/bin/phpize./configure --with-php-config=/usr/local/webserver/php/bin/php-configmakemake install#最后，修改php.ini，加上（去除下面#号）#extension = "imagick.so"</pre>
  
  <p STYLE="TexT-inDenT: 2em;">
    查看phpinfo，如果有Imagick，则说明安装成功。
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    <img ALT="用Imagick替代php的GD库 - 边城浪子 - 记录人生轨迹,寻找人生航向!" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://img01.littz.com/portal/2009/06/1_200906040253561ZxwK.thumb.jpg" BORDER="0" TITLE="转：用Imagick替代php的GD库-强大~" />
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    详细安装可参考 Nginx 0.7.x + PHP5.2.9（FastCGI）搭建胜过Apache十倍的Web服务器（第5版）http://blog.s135.com/nginx_php_v5/
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    Imagick函数库资料太少，能参考的就是PHP官方手册http://cn2.php.net/imagick，但是无详细示例介绍。下面简单写了个实例，找了手册很长时间才做出来的，希望对大家有帮助。
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    原图http://test.studenthome.cn/imagick/b.jpg
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    按要求缩小图片尺寸、增加半透明边框，读入exif信息，按指定要求显示在图片上。
  </p>
  
  <p STYLE="TexT-inDenT: 2em;">
    php生成的图片http://test.studenthome.cn/imagick/imagick4.php
  </p>
  
  <pre> $towidth = '500'; $toheight = '700'; //设置图片调整大小时允许的最大宽度和高度 $sourcefile = './b.jpg'; //定义一个图像文件路径 //$image-&gt;writeImage('./b.jpg.bak'); //可以备份这个图片 $myimage = new Imagick( $sourcefile ); //读入该图像文件 $exifobject = my_exif( $myimage ); //自写函数，读取exif信息（拍摄数据），按自己的要求排列exif信息，返回对象 //$myimage-&gt;setImageFormat('jpeg'); //把图片转为jpg格式 $myimage-&gt;setCompressionQuality( 100 ); //设置jpg压缩质量，1 - 100 $myimage-&gt;enhanceImage(); //去噪点 $sourcewidth = $myimage-&gt;getImageWidth(); //获取读入图像原始大小 if ( $sourcewidth &gt; $towidth ) {   $myimage-&gt;scaleImage( $towidth, $toheight, true ); //调整图片大小 } $myimage-&gt;raiseImage( 8, 8, 0, 0, 1 ); //加半透明边框 $resizewidth = $myimage-&gt;getImageWidth(); //读出调整之后的图片大小 $resizeheight = $myimage-&gt;getImageHeight(); $drawback = new ImagickDraw(); //实例化一个绘画对象，绘制半透明黑色背景给exif信息用 $drawback-&gt;setFillColor( new ImagickPixel('#000000') ); //设置填充颜色为黑色 $drawback-&gt;setFillOpacity( 0.6 ); //填充透明度为0.6，参数0.1-1，1为不透明 $drawback-&gt;rectangle( $resizewidth / 2 - 190, $resizeheight - 50, $resizewidth / 2 + 190, $resizeheight - 12 ); //绘制矩形参数，分别为左上角x、y，右下角x、y $myimage-&gt;drawImage( $drawback ); //确认到image中绘制该矩形框 $draw = new ImagickDraw(); //实例化一个绘画对象，绘制exif文本信息嵌入图片中 $draw-&gt;setFont( './xianhei.ttf' ); //设置文本字体，要求ttf或者ttc字体，可以绝对或者相对路径 $draw-&gt;setFontSize( 11 ); //设置字号 $draw-&gt;setTextAlignment( 2 ); //文字对齐方式，2为居中 $draw-&gt;setFillColor( '#FFFFFF' ); //文字填充颜色 $myimage-&gt;annotateImage( $draw, $resizewidth / 2, $resizeheight - 39, 0, $exifobject-&gt;row1 ); //绘制第一行文本，居中 $myimage-&gt;annotateImage( $draw, $resizewidth / 2, $resizeheight - 27, 0, $exifobject-&gt;row2 ); //绘制第二行文本，居中 $myimage-&gt;annotateImage( $draw, $resizewidth / 2, $resizeheight - 15, 0, $exifobject-&gt;row3 ); //绘制第三行文本，居中  header( 'Content-type: image/jpeg' ); //php文件输出mime类型为jpeg图片 echo $myimage; //在当前php页面输出图片 //$image-&gt;writeImage('./b.new.jpg'); //如果图片不需要在当前php程序中输出，使用写入图片到磁盘函数，上面的设置header也可以去除 $myimage-&gt;clear(); $myimage-&gt;destroy(); //释放资源 //自写函数，读取exif信息，返回对象 function my_exif( $myimage ) {   $exifArray = array( 'exif:Model' =&gt; '未知', 'exif:DateTimeOriginal' =&gt; '未知', 'exif:ExposureProgram' =&gt; '未知', 'exif:FNumber' =&gt; '0/10', 'exif:ExposureTime' =&gt; '0/10', 'exif:ISOSpeedRatings' =&gt; '未知',     'exif:MeteringMode' =&gt; '未知', 'exif:Flash' =&gt; '关闭闪光灯', 'exif:FocalLength' =&gt; '未知', 'exif:ExifImageWidth' =&gt; '未知', 'exif:ExifImageLength' =&gt; '未知' ); //初始化部分信息，防止无法读取照片exif信息时运算发生错误   $exifArray = $myimage-&gt;getImageProperties( "exif:*" ); //读取图片的exif信息，存入$exifArray数组   //如果需要显示原数组可以使用print_r($exifArray);   $r-&gt;row1 = '相机:' . $exifArray['exif:Model'];   $r-&gt;row1 = $r-&gt;row1 . ' 拍摄时间:' . $exifArray['exif:DateTimeOriginal'];   switch ( $exifArray['exif:ExposureProgram'] )   {     case 1:       $exifArray['exif:ExposureProgram'] = "手动(M)";       break; //Manual Control     case 2:       $exifArray['exif:ExposureProgram'] = "程序自动(P)";       break; //Program Normal     case 3:       $exifArray['exif:ExposureProgram'] = "光圈优先(A,Av)";       break; //Aperture Priority     case 4:       $exifArray['exif:ExposureProgram'] = "快门优先(S,Tv)";       break; //Shutter Priority     case 5:       $exifArray['exif:ExposureProgram'] = "慢速快门";       break; //Program Creative (Slow Program)     case 6:       $exifArray['exif:ExposureProgram'] = "运动模式";       break; //Program Action(High-Speed Program)     case 7:       $exifArray['exif:ExposureProgram'] = "人像";       break; //Portrait     case 8:       $exifArray['exif:ExposureProgram'] = "风景";       break; //Landscape     default:       $exifArray['exif:ExposureProgram'] = "其它";   }   $r-&gt;row1 = $r-&gt;row1 . ' 模式:' . $exifArray['exif:ExposureProgram'];   $exifArray['exif:FNumber'] = explode( '/', $exifArray['exif:FNumber'] );   $exifArray['exif:FNumber'] = $exifArray['exif:FNumber'][0] / $exifArray['exif:FNumber'][1];   $r-&gt;row2 = '光圈:F/' . $exifArray['exif:FNumber'];   $exifArray['exif:ExposureTime'] = explode( '/', $exifArray['exif:ExposureTime'] );   if ( ($exifArray['exif:ExposureTime'][0] / $exifArray['exif:ExposureTime'][1]) &gt;= 1 )   {     $exifArray['exif:ExposureTime'] = sprintf( "%.1fs", (float)$exifArray['exif:ExposureTime'][0] / $exifArray['exif:ExposureTime'][1] );   } else   {     $exifArray['exif:ExposureTime'] = sprintf( "1/%ds", $exifArray['exif:ExposureTime'][1] / $exifArray['exif:ExposureTime'][0] );   }   $r-&gt;row2 = $r-&gt;row2 . ' 快门:' . $exifArray['exif:ExposureTime'];   $r-&gt;row2 = $r-&gt;row2 . ' ISO:' . $exifArray['exif:ISOSpeedRatings'];   $exifArray['exif:Ex
posureBiasValue'] = explode( "/", $exifArray['exif:ExposureBiasValue'] );   $exifArray['exif:ExposureBiasValue'] = sprintf( "%1.1feV", ((float)$exifArray['exif:ExposureBiasValue'][0] / $exifArray['exif:ExposureBiasValue'][1] * 100) / 100 );   if ( (float)$exifArray['exif:ExposureBiasValue'] &gt; 0 )   {     $exifArray['exif:ExposureBiasValue'] = "+" . $exifArray['exif:ExposureBiasValue'];   }   $r-&gt;row2 = $r-&gt;row2 . ' 补偿:' . $exifArray['exif:ExposureBiasValue'];   switch ( $exifArray['exif:MeteringMode'] )   {     case 0:       $exifArray['exif:MeteringMode'] = "未知";       break;     case 1:       $exifArray['exif:MeteringMode'] = "矩阵";       break;     case 2:       $exifArray['exif:MeteringMode'] = "中央重点平均";       break;     case 3:       $exifArray['exif:MeteringMode'] = "点测光";       break;     case 4:       $exifArray['exif:MeteringMode'] = "多点测光";       break;     default:       $exifArray['exif:MeteringMode'] = "其它";   }   $r-&gt;row2 = $r-&gt;row2 . ' 测光:' . $exifArray['exif:MeteringMode'];   switch ( $exifArray['exif:Flash'] )   {     case 1:       $exifArray['exif:Flash'] = "开启闪光灯";       break;   }   $r-&gt;row2 = $r-&gt;row2 . ' ' . $exifArray['exif:Flash'];   if ( $exifArray['exif:FocalLengthIn35mmFilm'] )   {     $r-&gt;row3 = '等效焦距:' . $exifArray['exif:FocalLengthIn35mmFilm'] . "mm";   } else   {     $exifArray['exif:FocalLength'] = explode( "/", $exifArray['exif:FocalLength'] );     $exifArray['exif:FocalLength'] = sprintf( "%4.1fmm", (double)$exifArray['exif:FocalLength'][0] / $exifArray['exif:FocalLength'][1] );     $r-&gt;row3 = '焦距:' . $exifArray['exif:FocalLength'];   }   $r-&gt;row3 = $r-&gt;row3 . ' 原始像素:' . $exifArray['exif:ExifImageWidth'] . 'x' . $exifArray['exif:ExifImageLength'] . 'px';   if ( $exifArray['exif:Software'] )   {     $r-&gt;row3 = $r-&gt;row3 . ' 后期:' . $exifArray['exif:Software'];   }   return $r; }<br /><br />参考和原文<br />http://dsec.pku.edu.cn/~yuhj/wiki/ImageMagick.html<br />http://chenling1018.blog.163.com/blog/static/14802542010518102159792/<br /></pre>