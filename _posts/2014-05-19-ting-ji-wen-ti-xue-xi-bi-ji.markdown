---
layout: post
status: publish
published: true
title: 停机问题学习笔记
author:
  display_name: MForever78
  login: MForever
  email: shunjian1128@gmail.com
  url: ''
author_login: MForever
author_email: shunjian1128@gmail.com
wordpress_id: 672
wordpress_url: http://www.mforever78.com/blog/?p=672
date: '2014-05-19 10:36:10 +0800'
date_gmt: '2014-05-19 02:36:10 +0800'
categories:
- CS
tags:
- 费曼技巧
comments: []
---
<p><img src="https://o35qhjvld.qnssl.com/turing_machine.jpg" alt="Turing Machine" /></p>
<p>前段时间看到 Scott H Young 那篇介绍费曼技巧<a href="#fn:1" id="fnref:1" title="see footnote" class="footnote">[1]</a>的文章，一下想到了程序员界流传甚广的小黄鸭调试法（Rubber Duck Debugging）<a href="#fn:2" id="fnref:2" title="see footnote" class="footnote">[2]</a>。几天前晚上散步的时候，聊天儿时提起了罗素悖论，让我联想到图灵对停机问题精妙的解法。想拿这个试试费曼技巧，讲到一半发现卡壳了，自己把自己绕了进去。我想到刘未鹏说过的那种状态，学习一个算法，初看觉得太牛逼了，然后花十分钟理解了人家花了十年才想出的成果，再花十天完全忘记，只能想起那个算法十分精妙，该不会的还是不会。所以应该由本溯源地学习数学和算法，并且写一个博客记录下学习的过程。于是这篇就作为费曼技巧和写作学习实践的第一篇。</p>
<p>在计算机科学领域，最广为人知的问题是关于 P 与 NP 问题的讨论，但不论 P 还是 NP，它们只是对解决问题效率的讨论。其实还有一类问题，它们难到计算机永远无法解出，这类问题被称作不可判定问题（Undecidable Problem）<a href="#fn:3" id="fnref:3" title="see footnote" class="footnote">[3]</a>，其中一个可能是最有名的问题就是停机问题。</p>
<p>停机问题提出了这样的疑问：是否存在一个程序，可以判定任何一个程序是否会停止。</p>
<p>要解决这个问题，我们先假设存在一个程序满足上述条件，比如：</p>

{% highlight C %}
int yourBrilliantProgram(char * program, inputType  input){
    if (program(input) stops)
        return 1;
    else //It loops forever
        return 0;
}
{% endhighlight %}

<p>其中，第一个 <code>if</code> 语句中的判断是人类智慧的结晶，它用了神奇的方法得知了 <code>program(input)</code> 是否停止。</p>
<p>接着我们就要进行破坏了，再来考虑这样一个程序：</p>

{% highlight C %}
int myDisgustingProgram(char * program){
    if (yourBrilliantProgram(program, program)){
        while(1);
        return 0; //Never reaches here
    }
    else
        return 1;
}
{% endhighlight %}

<p>我的程序专门跟你作对，如果你说这个程序可以正常退出，那么我就作一个死循环；你说它是死循环的，我就正常退出给你返回一个<code>true</code>。</p>
<p>So far so good. 最精彩的地方来了，考虑 <code>yourBrilliantProgram(myDisgustingProgram, myDisgustingProgram)</code> 的返回值。这里有点绕，我们一步一步跟踪。</p>
<p>首先，进入 <code>yourBrilliantProgram</code> 的第一个判断语句，这里执行 <code>myDisgustingProgram(myDisgustingProgram)</code> ，进入 <code>if(yourBrilliantProgram(myDisgustingProgram, myDisgustingProgram))</code> 这一句。这里可能出现两个结果：</p>
<ul>
<li>返回 <code>1</code>，说明 <code>myDisgustingProgram(myDisgustingProgram)</code> 正常退出，那么当前程序进入死循环，此时上一层的 <code>yourBrilliantProgram()</code> 应该检测到这个死循环，并返回 <code>0</code>，说明 <code>myDisgustingProgram(myDisgustingProgram)</code> 是个死循环。矛盾。</li>
<li>返回 <code>0</code>，说明 <code>myDisgustingProgram(myDisgustingProgram)</code> 死循环，那么当前程序返回 <code>1</code> 并且正常退出，又证明 <code>myDisgustingProgram(myDisgustingProgram)</code> 不是死循环。矛盾。</li>
</ul>
<p>也就证明，根本不存在这样的程序可以判定任何一个程序是否停止。</p>
<div class="footnotes">
<hr />
<ol>
<li id="fn:1">
<a href="http://select.yeeyan.org/view/94114/329073">Scott H Young 如何在十天内掌握线性代数</a> <a href="#fnref:1" title="return to article" class="reversefootnote">&#160;&#8617;</a>
</li>
<li id="fn:2">
<a href="http://zh.wikipedia.org/wiki/小黄鸭调试法">小黄鸭调试法</a> <a href="#fnref:2" title="return to article" class="reversefootnote">&#160;&#8617;</a>
</li>
<li id="fn:3">
<a href="http://zh.wikipedia.org/wiki/不可判定问题列表">不可判定问题列表</a> <a href="#fnref:3" title="return to article" class="reversefootnote">&#160;&#8617;</a>
</li>
</ol>
</div>
