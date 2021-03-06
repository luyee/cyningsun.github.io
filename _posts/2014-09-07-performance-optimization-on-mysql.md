---
layout: post
title: Mysql性能优化小结
category: 后台技术
tags: MYSQL
---

### 如何做性能优化？
做好性能优化应该理解什么是性能优化。按照我的理解性能就是“从请求发起到得到响应所需要的时间”，而优化就是目标，是为了降低响应时间。

#### 1.选定优化目标(what)   
为了在有效的时间内做好性能优化达到成本与结果成正比，优化哪些目标同样重要。实际工作中所有的时间都是有成本的，理清轻重缓急与主次相当重要。根据阿姆达尔，对于一个时间比重占用不超过5%的查询进行优化，无论如何努力，收益也不会超过5%。例如，此次优化的选择：先用户侧后商户侧(前者对响应时间比较敏感)，先黄金路径接口后功能路径接口。
#### 2、如何进行优化    
如果目标是降低响应时间，那么首先我们要弄清楚“时间都去哪儿了”，对症下药的方案也就比较明了。完成一项任务所需的时间一般分为两个部分：执行时间和等待时间。  
如果想优化等待时间，最好的办法是测量和定位不同子任务花费的时间，然后选择一些耗时不正常的子任务优化。优化等待时间意味着等待是如何发生的，等待可能是由于不同子任务或者子系统间接影响导致，也可能由于磁盘I/O、CPU资源争抢、网络传输。更深入一点，如果业务程序是CPU消耗性的，可能系统调用也会影响响应时间。

#### 总结一下：
> 1.优化值得优化的查询，优化成本大于收益时就应该停止。   
> 2.弄清“时间都去哪儿了”，有的放矢

另外，有时候平均响应时间很正常，但却掩盖了某些异常响应。举例说“医院所有病人的平均体温没有任何价值”。被掩盖的细节里能提供更多的信息。

### Mysql性能优化

Mysql性能优化同样基于以上原理进行，同时需要一些Mysql的基础知识。    
> 1.Mysql索引的特性   
> 2.Mysql工具explain、profile   
> 3.Mysql的锁机制   


### Mysql性能优化小结
最好的优化永远是与业务关联的、最好的优化永远是良好的设计，所谓的优化只应该作为辅助和应对变更的方法，不应该把所有的任务放到优化阶段，不然所需要的消耗的时间将是指数级的！


(完)




