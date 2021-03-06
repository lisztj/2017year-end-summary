### 2017 年终总结暨 2018 年计划
<!-- ####  -->
<br />
<p>
  <small><a target="_blank" href="#">演讲人：赵健</a></small><br />
	<small>2017.12.01</small>
</p>



## [2017年度工作概述]()


## [参与项目]()


- 智运通
- 来货拉运维平台
- 货准达
- 大数据平台
- 公共服务平台
- 东风农机
- 斯诺官网改版



## [工作完成的具体情况]()


## [智运通]()

- 智运通 <!-- .element: class="fragment" data-fragment-index="1" -->
	- 外接发货单/询价单，融云客服2.0升级，无车承运人迭代，接口标准化及JWT的登录验证，电子合同迭代等经历16次迭代
	- 落实到实现细节的 angular+typescript 实践，学到很多
	- 至今对我的编码实践有很重要的影响，奠定了一个好的技术观，至少能认清代码的孰好孰坏，能辨明技术发展的方向；


## [来货拉]()

- 来货拉项目 <!-- .element: class="fragment" data-fragment-index="2" -->
	- 前期并未直接参与，由来货拉组内成员完成，直接参与了双11H5活动迭代及监控管理


## [货准达项目]()

- csp微信版 <!-- .element: class="fragment" data-fragment-index="3" -->
	- 学习到项目起始进行合理架构和规划的重要性


## [大数据平台]()

- 飓风物流大数据平台 <!-- .element: class="fragment" data-fragment-index="3" -->
	- 前期由组内成员开发完成，直接参与了后期的debug维护及监控管理


## [公共服务项目]()

- 公共服务项目 <!-- .element: class="fragment" data-fragment-index="3" -->
	- 短信发送记录，定位基址记录，来货拉日志记录
	- 项目的监控管理


## [东风农机项目]()

- 东风农机项目 <!-- .element: class="fragment" data-fragment-index="2" -->
	- 询价单/导出，货物，客户，片区
	- 该项目功能较为简单，用了angular极速研发、快速上线


## [斯诺官网改版]()

- 斯诺官网改版 <!-- .element: class="fragment" data-fragment-index="3" -->
	- 技术选型，由组内成员开发完成，过程中监控管理



## [工作存在不足之处]()


## 思维的束缚

Note:
- 很多问题我们的处理方法都太朴素，都是拍脑袋就想出来的，其实很多问题社区已经有了经过长期实践考虑全面踩了很多坑才发展成熟的解决方案，但我们还是用拍脑袋想出来的方案而不去了解社区的优秀实践，费时费力浪费资源。 


## 缺乏计划性

Note:
区分「编译时」和「运行时」（至少有这样的理念）非常重要，编译时可以做很多重要的事情，比如生成自动化文档，进行代码的内省（编译型语言相对于解释型语言的一大优势）等等。静态站点生成器（Hexo）、我用 Reveal.js 做的这个 Slide 都是如此。Parser 无处不在。

原理上来说，就是在代码运行之前，对代码先进行解析，获取必要的信息。

项目里面可以怎么做：用 Controller 层的接口注释自动生成 Swagger 文档、自动生成接口参数的 Validator（最大程度的保证文档和代码的一致性，包括输出参数的简单 Validation，验证接口输出行为是否符合预期）、自动生成测试用例（甚至是全自动生成的测试）

测试还可以怎样做：用固定的（自己定义的）规范来书写测试用例（定义输入输出等等，和文档的定义是类似的，但是可以详细到具体的某一个测例内容），然后用自己写的 Parser 解析读入，再自动进行测试。今后只要维护这样一份类似于 YAML 或者 JSON 的纯粹的测例文件就可以里，Parser 是通用的。

这涉及到编译原理的一些知识，甚至可以自己定义发明一门语言。但是要注意编译（Compile）和解析（Parse）还是不同的。

解耦合是指：代码中的数据和逻辑分离，可以抽离出各个项目通用的 Parser，形成 SDK。


## 总结与反思的不足

Note:
Facebook 的，大概三个月前发布的 1.0 版本。

前端：大前端，包括移动端。调用链靠前的部分。

减少 HTTP 调用是指：多个数据集合的组合，一次性返回。简单定义，多个组合成一起的查询形成一个符合前端要求的返回。

减轻后端的负担：
	- 文档：定义一个全集即可，不必分各种难以描述的条件分别说明返回；
	- 参数验证、统一数据返回、序列化：因为定义了数据模型，可以由 GraphQL 的实现来完成；

Google+ API 的做法：通过请求参数的 fields 字段来告诉后端要哪些信息。


## 没有追求

Note:
以上是它最大的特点。

Express 是很轻量，用来学习了解一个 Web 框架是怎样运作的非常合适，但是要做一些业务复杂的大型项目实现复杂的需求，有很多东西都需要自己手动来实现。

CLI：针对 Workflow 有定制的 [yeoman](http://yeoman.io/)，很方便自动生成组件。


## 缺乏积极性



## [2018年计划]()


## [混合开发H5]()


## [sentry日志系统]()


## [gulp自动化]()


## [前端周边基础服务]()


## [nodeService、RESTfulAPI、SwaggerAPI文档等]()


###


## Thank You
