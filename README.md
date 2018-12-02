# 1 关于整体的架构
&#160; &#160; &#160; &#160; 其实是不太好的，截止目前，提出一些问题，如果有人看的话最好修改一下。

# 1.1 service层
&#160; &#160; &#160; &#160; 这里比较重要，可以看到，我的代码是将主要功能全部实现在controller里面，这是比较老的写法，
自然也非常不好。现在比较常用的写法是利用service层，将C层解耦出来。这样，C层只负责简单的逻辑——主要还是接收请求返回视图
等操作。而service层专门实现具体的方法。
&#160; &#160; &#160; &#160; 但是这个还是看情况，如果比我写的这个还要小的项目，想要快速开发（例如期末项目），完全可以
像我这样的写法，毕竟这样真的很快。但是仍然不推荐。

# 1.2 面向接口编程
&#160; &#160; &#160; &#160; 这个是必须的，面向接口编程看起来似乎是比较麻烦的，尤其是对于小项目而言，service层直接写
就完事了。这里是看是不是有合作了，如果有合作，接口就非常重要。接口的重要性就在于一定的暴露。只将接口暴露出来，开发的时
候不用管这个方法如何实现，只用使用就可以了。而对于接口方法的开发人员也是直接实现方法就行，不用管在哪里调用。
&#160; &#160; &#160; &#160; 这种方式非常符合面向对象编程所追求的“高内聚低耦合”。而一般在service层，也是会对每一个
service都写一个接口。

# 1.3 dao层
&#160; &#160; &#160; &#160; dao层也是比较重要的，其实算是一种设计模式，跟数据库相关。在servlet编程使用JDBC的时候就
在使用了：应该将对数据库操作的方法写进dao层的接口，而dao层实现方法。而暴露出来的都是例如insertX、deleteX这样的方法，
从而保证在有数据库操作的时候，直接调用方法就能实现，而免去了重复代码的书写。

# 1.4 mybatis与dao层与数据源
&#160; &#160; &#160; &#160; 使用mybatis的时候，dao层应该有一定的修改：mapper接口需要写在dao层，而mapper的xml文件
一般也是放在dao层——但是总归不太好，很多专门开了一个recourse文件来放所有的xml。
&#160; &#160; &#160; &#160; 而可以看到我的代码：mybatis的sqlSession等对象的初始化在频繁调用。不得已甚至写成controller
的静态块。这样也是不好的，既然都使用spring了，其实很多也直接配spring-mybatis.xml来建立mybatis数据源，这样就能大量简
化代码。

# 2 内容
&#160; &#160; &#160; &#160; 整体上就是利用Spring MVC，来完成一个旅游网。大概就是分成2种角色，用户和管理员。用户可以
浏览旅游团信息，然后添加订单，确定行程。对于用户基本的增删改（没有查）。管理员可以管理旅游团，增删改，查其实就是获取所
有列表了。大概就是这样。很标准的入门。数据持久化是利用了MySQL，使用mybatis。前端部分感谢同学的帮助。

# 3 线程安全
&#160; &#160; &#160; &#160; 这里不太好，并没有实现线程安全。也是不涉及多线程的问题了

# 4 关于所用版本
* IDE是idea，2018.2，因为可能GitHub上有IDE的配置文件，所以可能直接fork下来idea能直接加载，这个没有试。
* Spring 框架是4.3，懒了，直接用idea下载的了。
* mybatis 是3.4.6
* MySQL 是5.7，直接fork的话需要考虑本地MySQL版本以及驱动版本。

# 4.1 jar包
&#160; &#160; &#160; &#160; 全部在WEB-INF/lib下。仍然需要注意MySQL的驱动