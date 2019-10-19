---
layout: post
date: 2019-10-10 20:56 0800
title: "pixel、分辨率、ppi、dpi、dp、dip区别及转换"
tags: ["像素"]
ext-js: https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML
---



屏幕尺寸（Screen Size）： 屏幕对角线的长度。iPhone5屏幕尺寸为4英寸、iPhone6屏幕尺寸为4.7英寸，指的是显示屏对角线的长度。 1 inch = 2.54cm = 25.4mm
分辨率：屏幕上的像素总数。常用的表现形式如：1280x720, 1920x1080等。

px，pixel，像素，电子屏幕上组成一幅图画或image的基本单元。
pt， point，点，印刷行业常用单位，等于1/72英寸。
ppi，pixel per inch，每英寸像素数，值越高，屏幕越细腻。
dpi， dot per inch，每英寸多少点，该值越高，则图片越细腻。
dp，dip， Density-independent pixel，安卓开发用的长度单位。以160ppi为标准，和iPhone的scale差不多的意思。安卓用dp适配，系统会自动将dp转换为px。当屏幕像素点密度为160ppi时，1dp=1px。

#### 一，pt与px ： 1pt = (ppi / 72)px。

当图片的分辨率是72ppi（dpi）时，1pt = 1px；
当图片的分辨率是72*2ppi（dpi）时，1pt = 2px；

#### 二，ppi与dpi：dpi=ppi
dpi最初用于衡量打印物上每英寸的点数密度，DPI值越大图片越精细。当DPI的概念用在计算机屏幕上时，就应称之为ppi。同理： PPI就是计算机屏幕上每英寸可以显示的像素点的数量。在电子屏幕显示中ppi和dpi是一样的。

#### 三，ppi计算方法
假设屏幕分辨率为W*H(px)，物理尺寸为a*b(inch)，
则我们常说的屏幕尺寸c（如5.0英寸）其实是对角线的长度，因此


$$
c = \sqrt{a^2 + b^2}
$$
​											

​											<u>勾股定理</u>



则像素密度（PPI），指的是屏幕单位长度的像素数


$$
DPI = PPI = W/a = H/b
$$
​											

​										<u>屏幕密度</u>

由此我们推理出：


$$
DPI^2 = PPI^2 = (W/a)^2 = (H/b)^2 = (W^2 + H^2)/(a^2 + b^2) = (W^2 + H^2)/c^2
$$


​									

​									<u>屏幕密度</u>

因此我们可以得出PPI（ DPI）计算公式：


$$
DPI = PPI = \sqrt{W^2 + H^2}/c
$$
​									

​									<u>PPI（ DPI）计算公式</u>

eg：iphone6分辨率1334*750px，尺寸4.7英寸

则其
$$
PPI = \sqrt{1334^2+750^2}/4.7 = 326
$$
​									

​										<u>iphone6像素密度</u>

#### 四，px和dp
dp，独立像素，虚拟单位，又称设备无关像素。1dp的长度相当于一个160dpi的屏幕上一个物理像素的长度。而160dpi的屏幕则是被android定义为基准的屏幕（mdpi）。在app运行的时候，android会将dp转为实际像素进行布局。转换的公式为：
$$
px = dp * (160 / dpi)。
$$



dp为安卓开发时的基本长度单位，根据不同的屏幕分辨率，与px有不同的对应关系。根据其像素密度，我们将安卓端屏幕分为以下几种规格：

 ![img](/img/post-imgs/2019-10-10-px-ppi-dpi-dp-01.png)



1dp即为当屏幕密度值为160ppi时，1pt=1px。则，在上表中，当密度为mdpi时，1dp = 1px。 以mdpi为标准，上表中屏幕的密度值比分别为：


$$
ldpi : mdpi : hdpi : xhdpi : xxhpi = 0.75 : 1 : 1.5 : 2 : 3
$$




即，在xhdpi的密度下，1dp=2px；在hdpi情况下，1dp=1.5px。其他类推。
