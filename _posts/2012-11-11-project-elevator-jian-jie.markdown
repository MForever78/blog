---
layout: post
status: publish
published: true
title: Project Elevator 简介
author:
  display_name: MForever78
  login: MForever
  email: shunjian1128@gmail.com
  url: ''
author_login: MForever
author_email: shunjian1128@gmail.com
excerpt: "<div style=\"text-indent:2em;font-family:微软雅黑;\">\r\n<h3 style=\"text-indent:0em\">概述（Overview）</h3>\r\n\r\n作为一个公司内专门设计和建造电梯系统的工程师，你最新的项目是为一个11层大楼的电梯设计一个高效的控制模块。电梯控制模块决定着电梯目前要去的层数和电梯目前的服务方向（即电梯要载去往上层还是下层的乘客）。\r\n\r\n"
wordpress_id: 480
wordpress_url: http://www.mforever78.com/blog/?p=480
date: '2012-11-11 23:53:43 +0800'
date_gmt: '2012-11-11 15:53:43 +0800'
categories:
- CS
tags:
- Project Elevator
comments: []
---

<h3 style="text-indent:0em">概述（Overview）</h3>
<p>作为一个公司内专门设计和建造电梯系统的工程师，你最新的项目是为一个11层大楼的电梯设计一个高效的控制模块。电梯控制模块决定着电梯目前要去的层数和电梯目前的服务方向（即电梯要载去往上层还是下层的乘客）。</p>
<p><a id="more"></a><a id="more-480"></a></p>
<h3 style="text-indent:0em">要求（Rules）</h3>
<ol style="text-indent:0em">
<li>对于每一个已经乘坐电梯乘客，都必须将其送到目标楼层。</li>
<li>电梯不能向乘客目标楼层相反的方向移动。</li>
<li>电梯只有在空着或送最后一批乘客时（将要空时）才能改变服务方向。</li>
<li>有三部电梯。</li>
<li>只能从下表提供的几个电梯模型中选择。
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/Model.png" border="0" alt="Model"/></p>
</li>
<li>三部电梯其中一个为货物电梯（载重&gt;=14）。</li>
<li>所有的电梯必须服务于所有的楼层。</li>
<li>当任何一部电梯当前无法被使用的时候，乘客必须仍然可以使用剩下的两部电梯到达任一楼层。</li>
<li>控制模块不能有自己的内部时钟，但可以拥有一个计时器。</li>
<li>电梯没有传感器来得到货物的重量以及当前电梯内乘客的数量。</li>
<li>每层楼高为4米。</li>
</ol>
<h4 style="text-indent:0em">分析：</h4>
<p>由规则1.2.可以知道，如果有乘客想去往某一楼层，则不能路过该楼层而不停下。</p>
<p>规则3.其实和1.2.是自恰的，如果电梯在仍有乘客时就改变了服务方向，必会违反1.或2.。</p>
<p>规则4.中所给出的电梯根据载客量可以分为货物电梯和正常电梯（参见规则6），而每一类中的每一部电梯都不处于完全劣势，因此无法直接排除选择任何一个。</p>
<p>规则7.废掉了那种「某部电梯只往返于OO楼和XX楼之间以优化平均时间」的想法。</p>
<p>规则8.是说不能有「所有电梯对乘客需求都置之不理来优化平均时间」的情况。</p>
<p>规则9.是说不能在规定的时刻用特定的算法，也就是不允许计对不同的scenario 设计程序。Timer 可以使用保证了不会有乘客被「饿死」的现象，即可以规定如果某人等待时长超过XX，则不顾一切先去接他。</p>
<p>规则10.是说无法预先得知楼层乘客的数量以及电梯内乘客的数量，从而不能根据这两个数据来判断是否在某一楼层停下。（比如电梯已经满员则不再接受任何乘客）</p>
<h3 style="text-indent:0em">前期准备（Preparation）</h3>
<p>电梯的模拟器已经写好，作为一个Project放出，你所要做的工作就是设计其中的控制模块。</p>
<p>电梯的其他重要模块以及接口简介：</p>
<h4 style="text-indent:0em">一、GUI</h4>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/GUI0.png" border="0" alt="GUI0"/></p>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/GUI1.png" border="0" alt="GUI1"/></p>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/GUI2.png" border="0" alt="GUI2"/></p>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/GUI3.png" border="0" alt="GUI3"/></p>
<h4 style="text-indent:0em">二、布署</h4>
<div style="text-indent:0em">
[java]<br />
// (1) Setup two elevators<br />
Elevator elevators[] = {<br />
  new Elevator(12, 6, 3.0),  new Elevator(12, 6, 3.0)<br />
};</p>
<p>// (2) Setup a building<br />
Building building = new Building(10, 4.0);</p>
<p>// (3) Setup a simulation stage<br />
SampleStage stage = new SampleStage(3000, 0);</p>
<p>// (4) Create an elevator controller<br />
BasicController controller = new BasicController();</p>
<p>// (5) Run a simulation with or without GUI<br />
Simulator.run(stage, building, elevators, controller, true);<br />
[/java]
</p></div>
<h4 style="text-indent:0em">三、建立电梯（Elevator）</h4>
<div style="text-indent: 0em;">
[java]<br />
// First, create an array of N elevators<br />
// (N = # elevators in the building)<br />
Elevator elevators[] = new Elevator [N];</p>
<p>// Then, assign each array element an Elevator object<br />
elevators[0] = new Elevator(12, 6.0, 3.0);<br />
[/java]
</p></div>
<p>参数的意义：</p>
<ul style="text-indent:0em">
<li>参数 #1：容量
<ul>
<li>即该部电梯的载客量。</li>
</ul>
</li>
<li>参数 #2：最大速度（单位 m/s）</li>
<li>参数 #3：加速度（单位 m/s^2）
<ul>
<li>即电梯速度的变化率。</li>
<li>电梯在保证能在目标楼层停下的情况下会自动加速至最大速度。</li>
</ul>
</li>
</ul>
<h4 style="text-indent:0em">四、建立建筑物（Building）</h4>
<div style="text-indent: 0em;">
[java]<br />
// Create a building object<br />
// (10 floors, and each floor is 4m in height)<br />
Building building = new Building(10, 4.0);<br />
[/java]
</div>
<p>参数的意义</p>
<ul style="text-indent:0em">
<li>参数 #1：总层数
<ul>
<li>包括第0层。</li>
</ul>
</li>
<li>参数 #2：层高（单位 m）
<ul>
<li>所有楼层高度相同。</li>
</ul>
</li>
</ul>
<h4 style="text-indent:0em">五、情景模型（Scenario Stage）</h4>
<p>一个情景模型控制着：</p>
<ul style="text-indent:0em">
<li>模拟器持续的时长
<ul>
<li>以时钟周期（clock tick）计算；1 clock tick = 0.1s</li>
</ul>
</li>
<li>乘客流动模型
<ul>
<li>控制乘客从X层至Y层请求的频率。</li>
<li>多位乘客可以在同一时刻发出去任一楼层的请求。</li>
</ul>
</li>
</ul>
<h4 style="text-indent:0em">六、电梯控制模块（Elevator Controller）</h4>
<p>电梯控制模块决定着电梯目前要去的层数和电梯目前的服务方向（即电梯要载去往上层还是下层的乘客）</p>
<h3 style="text-indent:0em">算法描述（Algorithm）</h3>
<h4 style="text-indent:0em">一、Basic Algorithm</h4>
<p>最简单的办法，对每一个电梯布署同样的控制模块，每次只上或下一层（现实中如果有这样的电梯一定会把人逼疯）。算法流程图如下。</p>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/BasicController.png" border="0" alt="BasicController"/></p>
<p>代码：</p>
<div style="text-indent: 0em;">
[java]<br />
import org.segonds.elevators.model.*;</p>
<p>public class BasicController implements Controller {<br />
    protected ControlledBuilding building;<br />
    protected ControlledElevator elevators[];</p>
<p>    private int buildingFloors, topFloor;<br />
    public final Direction UP = Direction.UP;<br />
    public final Direction DOWN = Direction.DOWN;<br />
    public final Direction UNCOMMITTED = Direction.UNCOMMITTED;</p>
<p>    @Override<br />
    public void setup(ControlledBuilding building) {<br />
        this.building = building;<br />
        elevators = building.getElevators();<br />
        buildingFloors = building.getNbFloors();<br />
        topFloor = buildingFloors - 1;<br />
    }</p>
<p>    @Override<br />
    public void reset() {<br />
        // nothing to do<br />
    }</p>
<p>    @Override<br />
    public String getName() {<br />
        return &quot;Basic Sweep Controller&quot;;<br />
    }</p>
<p>    @Override<br />
    public void tick(Clock clock) {</p>
<p>      // For each elevator<br />
      for (int i = 0; i &amp;lt; elevators.length; i++) {<br />
        ControlledElevator elevator = elevators[i];</p>
<p>        double speed = elevator.getSpeed();<br />
        int currentFloor = elevator.getFloor();</p>
<p>        // If the elevator is currently stopped at a floor and if its<br />
        // doors are opening.<br />
        if (speed == 0.0 &amp;amp;&amp;amp; elevator.isOpening()) {</p>
<p>          // If the elevator was moving up<br />
          if (elevator.getDirection() == UP) {</p>
<p>             // If the elevator is at the top floor, reverse its direction<br />
             if (currentFloor == topFloor) {<br />
               elevator.setDirection(DOWN);<br />
               elevator.setTarget(topFloor-1);<br />
             }<br />
             else  // Otherwise, continue to move up to the next floor<br />
               elevator.setTarget(currentFloor+1);<br />
          }<br />
          else<br />
          if (elevator.getDirection() == DOWN) {</p>
<p>             // If the elevator is at the ground floor, reverse its direction<br />
             if (currentFloor == 0) {<br />
               elevator.setDirection(UP);<br />
               elevator.setTarget(1);<br />
             } // Otherwise, continue to move down to the next floor.<br />
             else<br />
               elevator.setTarget(currentFloor-1);<br />
          }<br />
        }</p>
<p>        if (elevator.getDirection() == UNCOMMITTED) {<br />
          // If the elevator is idle. (initially)<br />
          elevator.setDirection(UP);<br />
          elevator.setTarget(0);<br />
        }<br />
      } // end for each elevator<br />
    }<br />
}<br />
[/java]
</p></div>
<h4 style="text-indent:0em">二、Baseline Algorithm</h4>
<p>上面的那个控制模块的优点是……好吧还真没什么优点。为了改进它，提出第二个控制模块，这也是现今大部分商场等电梯常用的控制程序，并没有什么特殊的优化，只是处理掉了Basic Algo的那些脑残特性。算法流程图如下。</p>
<p><img src="http://i1124.photobucket.com/albums/l569/shunjian1128/Website/Project%20Elevator/BaselineController.png" border="0" alt="BaselineController"/></p>
<p>代码：</p>
<div style="text-indent: 0em;">
[java]<br />
import org.segonds.elevators.model.*;</p>
<p>// Reference implementation of the Baseline Controller (as described in the<br />
// lecture note)<br />
public class RefBaselineController implements Controller {<br />
    /* Declaration of instance variables */<br />
    // The building object<br />
    ControlledBuilding building;<br />
    // An array of all elevator objects<br />
    ControlledElevator E[];</p>
<p>    // Number of floors<br />
    int nbFloors;<br />
    // Floor number of the top floor<br />
    int topFloor;</p>
<p>    // Set up shorter aliases<br />
    final Direction UP = Direction.UP;<br />
    final Direction DOWN = Direction.DOWN;<br />
    final Direction UNCOMMITTED = Direction.UNCOMMITTED;</p>
<p>    public void setup(ControlledBuilding b) {<br />
        // Save a reference to the building object for later use<br />
        building = b;</p>
<p>        // Obtain a reference to the array of elevators<br />
        E = building.getElevators();</p>
<p>       // Save these constants for easier access later<br />
        nbFloors = building.getNbFloors();<br />
        topFloor = nbFloors - 1;<br />
    }</p>
<p>    // Nothing to reset<br />
    public void reset() {}</p>
<p>    public String getName() {<br />
        return &quot;Baseline Controller&quot;;<br />
    }</p>
<p>    public void tick(Clock clock) {<br />
        // For each elevator<br />
        for (int i = 0; i &amp;lt; E.length; i++) {             // Elevator's velocity. speed == 0 =&amp;gt; Stopped;<br />
            // speed &amp;gt; 0 =&amp;gt; Moving up; speed &amp;lt; 0 =&amp;gt; Moving down<br />
            double speed = E[i].getSpeed();</p>
<p>            // Committed Direction<br />
            Direction CD = E[i].getDirection();</p>
<p>            if (CD == UP &amp;amp;&amp;amp; speed &amp;gt; 0)<br />
                handleCase1(i);<br />
            else<br />
            if (CD == UP &amp;amp;&amp;amp; speed == 0)<br />
                handleCase2(i);<br />
            else<br />
            if (CD == DOWN &amp;amp;&amp;amp; speed &amp;lt; 0)<br />
                handleCase3(i);<br />
            else<br />
            if (CD == DOWN &amp;amp;&amp;amp; speed == 0)<br />
                handleCase4(i);<br />
            else<br />
            if (CD == UNCOMMITTED)<br />
                handleCase5(i);<br />
        }<br />
    }</p>
<p>    public void handleCase1(int i) {<br />
        int nextFloor = E[i].nextStoppableFloor();</p>
<p>        // If floor x is a candidate, candidates[x] will be set to true<br />
        // Note: All elements are initialized to false by default.<br />
        boolean candidates[] = new boolean [nbFloors];</p>
<p>        // Locate candidates<br />
        for (int f = nextFloor; f = 0; f--)<br />
            if (building.isFloorRequested(f, DOWN) || E[i].isFloorRequested(f))<br />
                candidates[f] = true;</p>
<p>        // Initially use -1 to indicate &quot;target not found&quot;<br />
        int target = -1;</p>
<p>        // Find the highest candidate floor as the target<br />
        for (int f = topFloor; f &amp;gt;= 0; f--)<br />
            if (candidates[f] == true) {<br />
                target = f;<br />
                break;<br />
              }</p>
<p>        // If we can find a target<br />
        if (target != -1)<br />
            E[i].setTarget(target);<br />
        // Otherwise, treat the case as an idle case<br />
        else<br />
            handleCase5(i);<br />
    }</p>
<p>    public void handleCase4(int i) {<br />
        int currentFloor = E[i].getFloor();<br />
        if (currentFloor == 0) {<br />
            handleCase5(i);<br />
            return;<br />
        }</p>
<p>        // Nothing to do if the elevator doors are NOT in closed or<br />
        // closing states<br />
        if (! E[i].isClosing() )<br />
             return;</p>
<p>        int nextFloor = currentFloor - 1;</p>
<p>        boolean candidates[] = new boolean [nbFloors];</p>
<p>        for (int f = nextFloor; f &amp;gt;= 0; f--)<br />
            if (building.isFloorRequested(f, DOWN) || E[i].isFloorRequested(f))<br />
                candidates[f] = true;</p>
<p>        int target = -1;</p>
<p>        // Find the highest candidate floor as the target<br />
        for (int f = topFloor; f &amp;gt;= 0; f--)<br />
            if (candidates[f] == true) {<br />
                target = f;<br />
                break;<br />
            }</p>
<p>        // If we can find a target<br />
        if (target != -1)<br />
            E[i].setTarget(target);<br />
        // Otherwise, treat the case as an idle case<br />
        else<br />
            handleCase5(i);<br />
    }</p>
<p>    // Handle uncomitted case<br />
    void handleCase5(int i) {</p>
<p>        boolean candidates[] = new boolean [nbFloors];</p>
<p>        for (int f = 0; f = 0; f--)<br />
            if (candidates[f] == true) {<br />
                target = f;<br />
                break;<br />
            }</p>
<p>        if (target != -1) {<br />
            E[i].setTarget(target);<br />
            E[i].setDirection(DOWN);<br />
            return;<br />
        }</p>
<p>        // Otherwise stays idle<br />
        E[i].setDirection(UNCOMMITTED);<br />
    }</p>
<p>}<br />
[/java]
</p></div>
