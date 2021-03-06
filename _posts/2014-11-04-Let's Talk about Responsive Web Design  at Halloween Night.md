---
layout: post
title: 浅谈响应式设计
author: emma
categories: [home]
tags: [web,开源,响应式设计,responsive design,web front-end,web 前端]
fullview: false
---

&emsp;&emsp;Hello，今天是2014年10月31日，传说中的万圣节，于是想happy的谈一下这段时间对响应式设计的学习和理解。欢乐码字中。。。。。

##**What&Why** 

&emsp;&emsp;响应式网页设计最初是由 Ethan Marcotte 提出的一个概念：为什么一定要为每个用户群各自打造一套设计和开发方案？Web设计应该做到根据不同设备环境自动响应及调整。无论用户正在使用笔记本还是iPad，我们的页面都应该能够自动切换分辨率、图片尺寸及相关脚本功能等，以适应不同设备;换句话说，页面应该有能力去自动响应用户的设备环境。这样，我们就可以不必为不断到来的新设备做专门的版本设计和开发了。当然响应式Web设计不仅仅是关于屏幕分辨率自适应以及自动缩放的图片等等，它更像是一种对于设计的全新思维模式；我们应当向下兼容、移动优先。

##**How** 

**（1）**.	响应式设计要做的第一件事情就是在head标签里指定viewport meta 属性。

    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    
&emsp;&emsp;以上代码指定视口宽度等于设备宽度，初始缩放比例为 1 倍，也就是不缩放。

&emsp;&emsp;简单说来在手机（iPhone Safari）上访问网页时它默认会对网页进行缩放 ，尽可能多地在屏幕上展示整个页面的内容。而缩放之后的效果可想而知，一个在电脑上正常展示的页面被缩放进手机屏幕（通常是240*320）里面后，很难阅读，所以我们设定页面不会自动缩放。须记住：如果你的网站不是响应式的，请不要使用initial-scale或者user-scalable=no.

 **（2）**.	 让页面的内容自适应容器的可视部分（viewport）  
 
 一个页面需要兼容不同终端，那么有几个个关键点是我们需要去做到响应式的：
  
1）.响应式布局：我们可以监测页面布局随着不同的浏览环境而产生的变化，如果它们变的过窄过短或是过宽过长，则通过一个子级样式表来继承主样式表的设定，并专门针对某些布局结构进行样式覆写。
        
 例如：
 
 ![图1](http://i.imgur.com/bE2cEb8.png)
  
 ![图2](http://i.imgur.com/SCBZw2T.png)

    .our-team {
       padding: 1.18em 0;

      .profile {
       width: 33.3333%;
       padding: 0 2%;
       text-align: center;
       margin: .5em auto 2.5em;
       float: left; 
      }
    }

    @media only screen and (max-width:1024px) {
		.our-team {
    	   .profile {
     	    width: 50%;
			} 
 		}
   	}
   	
&emsp;&emsp;这两段代码是不完整的，只截取了问题相关部分，放在about.less中。第一段为部分主样式，第二段的代码的width属性对第一段进行了覆写，其它的属性全部继承。当屏幕分辨率小于580px，即用手机大小的屏幕进行浏览时，我们可以设置宽度为100%，使得一张图片占一行。
 
2）使用相对单位：响应式设计的一个关键点就是流动性、比例性，而非固定的宽度布局或文字大小，所以我们应尽量使用相对单位。
    
**NO**

	div.fullWidth {                                                    
	   width: 320px;
	   margin-left: auto;
       margin-right: auto;
	}

**YES**

	div.fullWidth {                                                    
	   width: 100%;
	}   
	
&emsp;&emsp;对于字体来说，CSS中比较常用的指定字体相对大小的单位有百分比，em及CSS3新增的rem，避免指定固定像素的字体大小，可以使我们的字体更有弹性。

&emsp;&emsp;对于布局来说，使用相对单位可以防止元素呈现在视口中过小或过大（从而产生横向滚动条）。比如：在一个顶级div上设置width:100%，这样不管是320px宽的iPhone，还是342px宽的蓝莓Z10，这个div会恰好铺满整个容器可视部分（viewport）的宽度。此外，相对单位使得浏览器基于用户的缩放（zoom level）渲染内容，而不需要添加水平滚动条。
      
3) 弹性图片：通过max-width: 100%和height：auto实现。
         
如下：

	img {
	   max-width: 100%;
	   height: auto;
	}

&emsp;&emsp;普通的图片是不会自适应屏幕大小的，也就是说图片太宽的话在手持设备等屏幕较小的情况下会有水平滚动条出现。视觉效果可以参考上一个例子的图片。  

4) 显示或隐藏内容： 
    
    display:none;

&emsp;&emsp;这行样式代码在响应式设计中很重要，它用于隐藏某块内容。

![图3](http://i.imgur.com/AWcZfns.png)
 
![图4](http://i.imgur.com/rjKmy8Q.png)

&emsp;&emsp;可以看到，图3中的中英文切换跟菜单栏在图4中没有显示，这里就是当用手机浏览时，我们把有些不合适的内容隐藏了，图3中的菜单栏没有输出，在图4的右上角重新设计了一个适合手机浏览的菜单显示。

（3） CSS3 media queries的使用

 &emsp;&emsp;媒体查询是响应式设计的核心，它是使用CSS根据分辨率宽度的变化来调整页面布局结构，从而使特定的风格应用于小屏，大屏及介于两者之间。对于那些尚不支持media query的浏览器（IE8或是之前的版本），我们要在页面中调用css3-mediaqueries.js。

     @media (query)  {
     /* CSS used when query matches*/
    }

&emsp;&emsp;尽管我们可以使用很多属性查询，但在响应式设计中，最常用的还是`min-width，max-width，min-height，max-height` 。

	@media only screen and (max-width:1024px)
	{.................}
	@media only screen and (max-width:768px)
	{.................}
	@media only screen and (max-width:580px)
	{.................}

首先当 `width>1024px` 时，默认主样式会起作用。

 - 当`width`大于768px小于1024px时，`max-width:1024px`所定义的风格将会起作用，它对主样式进行继承和覆写，从而对页面内容进行渲染
 - 当width大于580px小于768px时，`max-width:768px`及`max-width:1024px`的样式都会起作用，它们之间存在继承和覆写，从而对页面内容进行渲染。
 - 当width小于580px时，以上三个样式都会起作用，同样根据声明的前后次序，对主样式进行继承和覆写。

（4） 怎样选择断点（breakpoints）

&emsp;&emsp;可以看到，上面的**media query**代码选取了三个断点来应用不同的风格，580px，768px，及1024px，那么这几个点到底是基于什么选取呢？

&emsp;&emsp;一般我们刚开始做响应式设计，都是基于设备去选取断点的，通常是按照智能手机（iPhone是320\*480），平板（iPad是768\*1024），然后就是所有大于1024px的屏幕。但是这样容易出现后期维护问题，现在的设备分辨率各有不同，而且以后会出现神马样的谁也无法预料，所以基于设备去选择断点还是不那么靠谱！那么又该依据神马呢？

&emsp;&emsp; 答案是：基于网站本身的内容。

&emsp;&emsp; 我们可以先从小到大，从手机端开始走起。首先，让网页内容风格适合小屏幕，然后不断地扩大，当我们的内容在达到一个临界点后，视觉效果不符合人们的审美或影响了内容获取时，这就是我们需要的断点，这样可以确保所设的断点数最少。但是我们可能无法在视觉设计的阶段就能覆盖其尺寸区间内容所有状况，这样我们就需要把它和现有的设备相结合确立断点。在这个过程中，如果整个页面只是某一点的内容看起来不协调，我们可以试着改变一下主CSS样式，而不至于为了这一丢丢不美观而加设一个断点，增加不必要的代码。
 