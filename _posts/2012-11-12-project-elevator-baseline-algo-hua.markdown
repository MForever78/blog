---
layout: post
status: publish
published: true
title: Project Elevator：Baseline Algo 优化
author:
  display_name: MForever78
  login: MForever
  email: shunjian1128@gmail.com
  url: ''
author_login: MForever
author_email: shunjian1128@gmail.com
wordpress_id: 534
wordpress_url: http://www.mforever78.com/blog/?p=534
date: '2012-11-12 13:05:51 +0800'
date_gmt: '2012-11-12 05:05:51 +0800'
categories:
- CS
tags:
- Project Elevator
comments: []
---

<p>对于BaselineController，其实还是有很多缺点。我分条列出，一个一个想解决的方法。</p>
<ol style="text-indent:0em;">
<li>Case5的处理方法不好。只是简单地寻找请求楼层，并没有根据电梯当前所在的位置进行优化。</li>
<li>当电梯空闲的时候，不进行动作。</li>
<li>对于两部电梯使用了同一套控制程序，不能很好地搭配，因而出现了大量冗余操作。</li>
<li>没有针对不同的交通状况设计不同的算法。</li>
</ol>
<p>对于1. 优化方案是找距离电梯最近的楼层服务。而这样做没有考虑到服务方向的问题，因此可以使其中一个电梯优先服务向上的乘客，另一个电梯优先服务向下的乘客。</p>
<p>对于2. 可根据当前的交通状况设计算法“猜”可能出现乘客的楼层，提前运行。</p>
<p>对于3. 防止两部电梯出现同时响应同一个请求的现象，根据具体情况选择优先级高的电梯进行服务。</p>
<p>对于4. 因为无法根据时间判断现在的交通情景，所以想到设计一个算法，根据当前的交通状况，让程序自行判断情景，然后进入相应设计的算法中。</p>
<p>前面的问题其实都是小问题，只有最后一个比较难解决。对于不同的交通情景，如何精确地抓住其特征，并抽象出数学模型，用算法的形式体现出来，这是难点。对于这一个问题，想到引入「模糊数学」的模型解决，后面再谈。</p>
<p>除了上面的这些之外，可以制定一些电梯调度原则，对任何情景都适用，我想到下面的几条</p>
<ol style="text-indent:0em">
<li>最小等待时间原则：控制模块制定一台电梯响应召唤时，应使乘客待梯时间最小。</li>
<li>最短距离调度原则：将各个层站的召唤信号分配给响应这一召唤信号最近的那台电梯。计算距离时，对同向和反向的召唤信号给予一个不同的位置偏差。</li>
<li>优先调度原则：在待梯时间不超过规定值时，对某层的召唤，由已经接受该层内指令的电梯应召。</li>
<li>已启动电梯优先原则：在不会使乘客待梯时间过长的前提下，优先使用已启动的电梯。</li>
<li>固定分区调度原则：
<ul>
<li>即按电梯台数和建筑物层数分成相应的运行区域。当无召唤时，各台电梯停靠在自己所服务区域的首层。</li>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/Partition.png" border="0" alt="Partition"/></p>
<li>在15层的建筑物中有3台电梯为其服务，其中A梯负责响应1一5楼乘客的召唤信号，B梯负责响应6一10楼乘客的召唤信号，C梯负责响应11一15楼乘客的召唤信号。当某个区域中有召唤信号时，由该区域的电梯去响应。但每台电梯的服务区域并非固定不变，根据召唤信号的不同可以随时调整其服务的区域。例如，A梯由于响应召唤而进入第二分区，而B梯则由于响应召唤进入第一分区，则此时A梯就成为第二分区服务的电梯，而B梯则服务于第一分区。若出现两梯运行到同一区域，如A梯进入第二分区，此时B梯仍在第二分区内，则A梯返回它原先服务的区域内服务。</li>
</ul>
</li>
</ol>

