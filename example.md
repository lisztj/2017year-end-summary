### 2017 年终总结暨 2018 年计划
<!-- ####  -->
<br />
<p>
  <small><a target="_blank" href="#">演讲人：赵健</a></small><br />
	<small>2017.12.01</small>
</p>



## 2017年度工作概述   


## [参与项目]()

- 大数据平台
- 来货拉运维平台/H5双11活动
- 公共服务平台
- 智运通
- 东风农机
- 货准达
- 斯诺官网改版


## [智运通]()

- 智运通 <!-- .element: class="fragment" data-fragment-index="1" -->
	- 外接发货单/询价单，融云客服2.0升级，无车承运人迭代，接口标准化及JWT的登录验证，电子合同迭代等经历16次迭代
	- 落实到实现细节的 angular+typescript 实践，学到很多
	- 至今对我的编码实践有很重要的影响，奠定了一个好的技术观，至少能认清代码的孰好孰坏，能辨明技术发展的方向；


## [东风农机项目]()

- 东风农机项目 <!-- .element: class="fragment" data-fragment-index="2" -->
	- 询价单/导出，货物，客户，片区
	- 该项目功能简单，用了angular极速研发、快速上线


## [货准达项目]()

- csp微信版 <!-- .element: class="fragment" data-fragment-index="3" -->
	- 项目起始进行合理架构和规划的重要性



## [工作完成的具体情况]()


## Objectives

- Indirection & pipeline
- RESTful & HTTP(S)
- Validation, document & auto-test
- Security & throttling
- Introspection
- Scalable
- ……

Note:
首页确立这些基本目标，细节后面再讲：
- （横向）分层良好 &（纵向）pipeline 清晰
- 对协议标准严格贯彻
- 要有验证，也要有自动化文档和自动化测试
- 安全和访问控制也要有
- 内省，日志收集、项目状态、API状态、自我监控等等
- 可扩展


## 工作不足之处


![简单项目架构图]()


![复杂项目架构图](resource/img/architecture2.png "复杂项目架构图")
<small class="fragment">图片来源：<a href="https://zhuanlan.zhihu.com/p/20691649">https://zhuanlan.zhihu.com/p/20691649</a></small>

Note:
与上图的区别在于划分出各个子系统，符合微服务的理念，各个子系统之间使用 RPC 进行调用。


## Process Pipeline

<img data-src="resource/img/process-pipeline1.png" alt="Down arrow" class="fragment">
<small class="fragment">图片来源：<a href="https://zhuanlan.zhihu.com/p/20691649">https://zhuanlan.zhihu.com/p/20691649</a></small>

Note:
ACL = Access Control List
先简要过一下，后面再细讲


## Protocol Done Right

- REST 
  - GET, POST, PUT, PATCH, DELETE
<br /><br />
- HTTP Headers
	- Accept(content type negotiation), If-Modified-Since/If-None-Match, If-Match/If-Modified
<br /><br />
- HTTP Status Code
	- 2XX, 3XX, 4XX, 5XX

Note:
- PUT 替换，PATCH 修改
- 根据请求头 Accept 返回相应的返回类型（xml or json...），后面的对于客户端缓存（304）或者对资源并发修改有效
- 正确返回 Status Code，这样同样 Protocol Done Right 的客户端才知道怎么正确处理你的返回或者接下来应该做什么


## Security & Validation

- Authentication
	- [使用 token（JWT）而不是 Session 进行验证](https://float-middle.com/json-web-tokens-jwt-vs-sessions/)
<br /><br />
- Authorization & ACT(Access Control List)
	- **什么人** 可以做 **什么事**
  - HTTP BASIC Auth，OAuth，HMAC Auth......
<br /><br />
- Data Validation
	- 请求的 Header, URL, Body 及其参数类型、范围、长度等等都是否合法
<br /><br />
- HTTPS

Note:
- 验证，JWT = JSON Web Tokens
	- 好处：不用存储 Session 也不用对处理 Session 的垃圾回收（减少服务端开销）、更易扩展（Session 必须得存储在一个公共访问的地方，或者做同步，提高了扩展的成本）、是真正符合 REST 协议的（接口无状态）、cookie 被截获容易受到 CSRF（跨站请求伪造）攻击
	- 对参数进行了编码、加密、签名达到验证用户身份或者说状态的目的
- 授权 & 访问控制列表
	- 实现复杂的接口访问控制，比如：某接口只允许中国用户在固定的时间段访问
- 数据校验
- HTTPS ／ HTTP2


## Conditional Request & Throttling

- Conditional Request
	- If-Match/If-Modified(If-None-Match/If-Not-Modified)
	- 作用：
		- 304
		- 数据改动的并发控制
<br /><br />
- Throttling
	- 防止 DOS 和 Replay 攻击

Note:
- 条件访问
- Throttling
	- 接口访问频率控制，可以用上张 PPT Authorization 的协议实现（字段加时间戳），也可以参考梁学长的分享
	- 重放攻击（将时间戳计算在内）


## Normalization

- Request
	- Client Adapter
	- Paginator
	- Input Data Adapter
<br /><br />
- Response
	- Output Data Adapter
	- Aliasing
	- Partial Response

Note:
- 请求
	- 客户端相关的信息（比如 device ID、platform、IP……）组成一个 Adapter，在固定的地方处理
	- 翻页 Adapter，limit ／ offset
	- 其他输入数据的 Adapter，比如输入数据字段的命名风格和代码里面风格不一致，可以在这里统一适配
- 返回
	- 返回数据适配，比输入数据适配承担更多的功能：多个数据表信息的组合、同一类信息输出格式风格的统一、命名风格转换等
	- 字段别名（数据库字段的重命名），统一在某处处理
	- 部分返回，只返回客户端需要的，之后还会再细讲


## Hooks

- preprocessing 
- postprocessing
<br /><br />
- Koa 的「洋葱体」结构

Note:
对于一些接口的特殊业务需求，可以允许添加 hook 进行扩展，入口和出口处都可能有。
感觉可以和 Koa 的洋葱体的 Process Pipeline 稍微类比一下。


## 工作存在不足之处

- Web Framework 
	- Express/FeathersJS/Hapi/Restify, Django/Flask, Plug/Phoenix……
<br /><br />
- ORM 
	- Sequelize, Mongoose, Waterline……
<br /><br />
- Document 
	- [Swagger](http://maples7.com/2016/09/06/build-doc-system-of-express-api-server-with-swagger/), RAML, API Blueprint, apidoc……
<br /><br />
- Data Validation 
	- Swagger 插件, joi, JSON Schema……

Note:
- JS、Python、Elixir
- Waterline 的优势在于同时支持各种主流的关系型和非关系型数据库，这样代码与数据库类型无关，要换数据库几乎不用改动代码，达到解耦的目的
- Swagger 虽然上手门槛相对高一些，但是很强大，形成了生态，可以围绕文档做很多附加的事情（比如参数验证、自动化测试，后面细讲），建议使用
- joi 是 Hapi 提供的 Validator，没有 JSON Schema 辣么啰嗦。如果不以 Swagger 为中心，还有一条线可以选择：joi 逆向生成 JSON Schema，然后用其生成 Swagger 文档，进而还可以尝试生成自动化测试


## 技术选型

- Test Framework 
	- ava/rewire/supertest/nyc
	- mocha/should(chai)/supertest/istanbul
<br /><br />
- Log System
	- ELK(ElasticSearch + Logstash + Kibana), bunyan, log4js, morgan……
<br /><br />
- Message Queue
  - kafka, rabbitMQ……
<br /><br />
- ……

Note:
- ava 更(gèng)新，可以并发执行、效率很高，而且对 ES6 支持很好
- ELK：ElasticSearch 负责实时全文检索、Logsash 负责日志转发（所以把所有项目日志放在服务器同样的目录下完全没有必要。很多问题我们的处理方法都太朴素，都是拍脑袋就想出来的，其实很多问题社区已经有了经过长期实践考虑全面踩了很多坑才发展成熟的解决方案，但我们还是用拍脑袋想出来的方案而不去了解社区的优秀实践，费时费力浪费资源。配置文件同理，重要的是做好权限管理，而不是连开发都不能信任不让其知道数据库密码，没有信任什么事情都做不了）、Kibana 负责炫酷的数据可视化；bunyan：JSON 格式的日志；


## 「编译时」与「运行时」

<br />
- 自动化文档？输入参数验证？自动化测试？
<br /><br />
- 写一个 Parser！
	- RegExp?
	- Parser Generator: instaparse(Clojure), Jison(JavaScript), antlr4, parsec(Haskell)
<br /><br />
- 不要重复定义！最大程度保证文档和代码的一致性！还可以解耦合！

Note:
区分「编译时」和「运行时」（至少有这样的理念）非常重要，编译时可以做很多重要的事情，比如生成自动化文档，进行代码的内省（编译型语言相对于解释型语言的一大优势）等等。静态站点生成器（Hexo）、我用 Reveal.js 做的这个 Slide 都是如此。Parser 无处不在。

原理上来说，就是在代码运行之前，对代码先进行解析，获取必要的信息。

项目里面可以怎么做：用 Controller 层的接口注释自动生成 Swagger 文档、自动生成接口参数的 Validator（最大程度的保证文档和代码的一致性，包括输出参数的简单 Validation，验证接口输出行为是否符合预期）、自动生成测试用例（甚至是全自动生成的测试）

测试还可以怎样做：用固定的（自己定义的）规范来书写测试用例（定义输入输出等等，和文档的定义是类似的，但是可以详细到具体的某一个测例内容），然后用自己写的 Parser 解析读入，再自动进行测试。今后只要维护这样一份类似于 YAML 或者 JSON 的纯粹的测例文件就可以里，Parser 是通用的。

这涉及到编译原理的一些知识，甚至可以自己定义发明一门语言。但是要注意编译（Compile）和解析（Parse）还是不同的。

解耦合是指：代码中的数据和逻辑分离，可以抽离出各个项目通用的 Parser，形成 SDK。


## 推荐阅读

- [撰写安全合格的REST API](https://zhuanlan.zhihu.com/p/20034107)
- [再谈 API 的撰写 - 总览](https://zhuanlan.zhihu.com/p/20691602)
- [再谈 API 的撰写 - 架构](https://zhuanlan.zhihu.com/p/20691649)
- [再谈 API 的撰写 - 子系统](https://zhuanlan.zhihu.com/p/20691777)
- [再谈 API 的撰写 - 契约](https://zhuanlan.zhihu.com/p/20691806)
- [谈谈编译和运行](https://zhuanlan.zhihu.com/p/20691721)
- [如何愉快地写个小parser](https://zhuanlan.zhihu.com/p/20178871)



## 工作不足之处


## 

- 建立接口参数的数据模型，并让**前端**来决定它需要的信息
	- 所有返回数据的一个子集
	- 作用：
		- 减少 HTTP 调用，只返回需要的信息（尤其是对于移动端，节省流量很重要）
		- 减轻后端的负担（文档、参数验证、统一数据返回、序列化等）
<br /><br />
- Google+ API: [Partial Response](https://developers.google.com/+/web/api/rest/#partial-response)

Note:
Facebook 的，大概三个月前发布的 1.0 版本。

前端：大前端，包括移动端。调用链靠前的部分。

减少 HTTP 调用是指：多个数据集合的组合，一次性返回。简单定义，多个组合成一起的查询形成一个符合前端要求的返回。

减轻后端的负担：
	- 文档：定义一个全集即可，不必分各种难以描述的条件分别说明返回；
	- 参数验证、统一数据返回、序列化：因为定义了数据模型，可以由 GraphQL 的实现来完成；

Google+ API 的做法：通过请求参数的 fields 字段来告诉后端要哪些信息。


## [FeathersJS](https://feathersjs.com/)

- Realtime: Socket.io, Primus
<br /><br />
- _Very Easy_ to use, compared with Express
	- Unfair, it's based on Express and have good **encapsulation** and **CLI**
<br /><br />
- 对关键问题的处理方法十分标准
	- the most important thing!

Note:
以上是它最大的特点。

Express 是很轻量，用来学习了解一个 Web 框架是怎样运作的非常合适，但是要做一些业务复杂的大型项目实现复杂的需求，有很多东西都需要自己手动来实现。

CLI：针对 Workflow 有定制的 [yeoman](http://yeoman.io/)，很方便自动生成组件。


## Yarn

- Fast with Cache and Parallel installation
<br /><br />
- Precise
<br /><br />
- Compatible
  - the most successful point!

Note:
- Cache：除了更快还可以有离线模式
- Precise：验证包的完整性，也会记录包的可靠来源，保证对于同一份 yarn.lock 和 package.json 下到的包完全一致。
- Compatible：对于现有的 NPM 项目是兼容的（直接 yarn 即可），对于 Node 解析包的机制也是兼容的。


## Erlang/Elixir/Phoenix

- Very Well-Designed language
<br /><br />
- Functional Promgamming
	- Neat, clean and graceful
<br /><br />
- Born Concurrency, Pattern Matching, Hot Reload
<br /><br />
- 强大的工具链

Note:
Elixir 兼容 Erlang 虚拟机。

- 函数式编程：JavaScript 其实只是学习了一些表面的函数式编程的技巧，比如函数是一等公民，但本质上有很多函数式编程原则上的要求 JavaScript 是不符合的，比如典型的 不可变状态，在 JS 里轻易就可以破坏这个本质要求，但在 Erlang 里则是有这些约束的（变量只能绑定一次）。
- Born Concurrency：面向并行、actor 并发模型（不同于 Node.js 单线程的 event-loop 的并发模型，这甚至可以说是并行模型的两个极端，但是 actor 模型写出的代码来反映世界的并行性可能更为清晰和易于理解（Erlang 里的并发是由互相通信的多组顺序进程（Erlang 进程，不是操作系统进程）组成的，没有复杂的互斥、锁等这些概念）；而且 Erlang 的并发是语言本身的 Erlang 虚拟机提供的，与操作系统或任何外部库无关，这意味着在不同操作系统上的并发是可靠且行为一致的）、适应 CPU 的多核化与云计算、let it crash（由于这种并发模型容错性变得特别好）、分布式
- Pattern Matching：不用 if-else 或者 Switch，跟同名函数的多态类似，更易扩展
- Hot Reload：可以在线重启系统的某一部分，跟移动端现在的热修复技术（年会那天有人分享这个）有类似之处

三言两语很难讲清楚，我其实了解得也还不深，建议自己去详细了解。但我觉得这个技术栈很可能成为下一个后端技术栈的风口。



## 推荐一些小工具


## PlantUML
<img data-src="resource/img/plantuml.png" alt="Down arrow">

Note:
优势：可以做版本管理、文本信息方便对比解析处理（本质上也是一个 Parser）


## [devdocs.io](http://devdocs.io/)

Note:
类似于 macOS 上的 Dash，文档的集合，还有版本管理，告别一大堆零散的浏览器书签


## [codelf](https://unbug.github.io/codelf/)
<br />
### There are only two hard things in Computer Science: cache invalidation and naming things.
#### —— Phil Karlton

Note:
注意右上角还可以管理你的 GitHub 上 Star 过的项目，可以分组、可以加标签等等。


## [node-modules.com](http://node-modules.com/)

Note:
Node 开发的一大被忽视的难题就是快速的在茫茫第三发库中找到自己满意的那一个，我曾经有一周大部分时间在找包和阅读各种文档，这个工具可以缓和这个问题。

搜的是 NPM 包，但是根据 GitHub 的 Star 数和你个人定制的 GitHub 信息（比如关注的人和 Star 的项目）来排序的，找得更快。


## Zsh, oh my zsh, babun

Note:
一个 Shell，普通青年用 Bash，文艺青年用 Zsh。非常强大的 Shell，可以尝试一下。

oh my zsh 来管理 Zsh 的配置。

Windows 上用 babun，默认的 Shell 是 Zsh。



## Transfer to ES6/7

Note:
已经讲了很多了，这部分简略过一下。


## const & let

- Use _const_ for all of your references; avoid using _var_.
- If you must reassign references, use _let_ instead of _var_.
- _typeof_ is no longer safe because of _Temporal Dead Zones (TDZ)_.

Note:
以前写 polyfill 打猴子补丁的时候似乎习惯用 typeof 来判断函数是不是存在，即便不存在也是返回 undefined，不会抛错。但是现在由于临时性死区这种方法已经不再安全了。


## Use computed property

```js
function getKey(k) {
  return `a key named ${k}`;
}

// bad
const obj = {
	id: 5,
	name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
	id: 5,
	name: 'San Francisco',
	[getKey('enabled')]: true,
};
```


## Use shorthand

```js
// bad
const atom = {
	value: 1,

	addValue: function (value) {
		return atom.value + value;
	},
};

// good
const atom = {
	value: 1,

	addValue(value) {
		return atom.value + value;
	},
};
						
// ---------------------------

const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
};

// good
const obj = {
  lukeSkywalker,
};
```

Note:
shorthand: 速记法，简略的表达方式。这里理解为更简洁的语法糖吧。


## Use rest operator

```js
// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }

// Note: see https://github.com/airbnb/javascript/issues/1430 about the syntax error

// ---------------------------

// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
	itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];

// ---------------------------

// bad
function concatenateAll() {
	const args = Array.prototype.slice.call(arguments);
	return args.join('');
}

// good
function concatenateAll(...args) {
	return args.join('');
}

// ---------------------------

// bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// bad
new (Function.prototype.bind.apply(Date, [null, 2016, 08, 05]));

// good
new Date(...[2016, 08, 05]);
```

Note:
rest operator: 展开运算符。

第三个例子：因为历史原因 arguments 只是类数组，没有 join 方法，所以要先 slice.call 转换成真正的数组，但是 ...args 就可以直接用。

最后一个例子：曾经遇到过这个问题，第三发库接口是那样设计的，我这边的数据放在数组里，用 Apply 不够简洁，用展开运算符就很方便。


## Destructuring

```js
// bad
function getFullName(user) {
	const firstName = user.firstName;
	const lastName = user.lastName;

	return `${firstName} ${lastName}`;
}

// good
function getFullName(user) {
	const { firstName, lastName } = user;
	return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
	return `${firstName} ${lastName}`;
}

// ---------------------------

const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;

// ---------------------------

// bad
function processInput(input) {
	// then a miracle occurs
	return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// good
function processInput(input) {
	// then a miracle occurs
	return { left, right, top, bottom };
}

// the caller selects only the data they need
const { left, top } = processInput(input);

// ---------------------------

// really bad
function handleThings(opts) {
	// No! We shouldn't mutate function arguments.
	// Double bad: if opts is falsy it'll be set to an object which may
	// be what you want but it can introduce subtle bugs.
	opts = opts || {};
	// ...
}

// still bad
function handleThings(opts) {
	if (opts === void 0) {
		opts = {};
	}
	// ...
}

// good
function handleThings(opts = {}) {
	// ...
}
```

Note:
1. 对象的解构；
2. 数组的解构；
3. 函数返回一组数据用对象而不是用数组（调用参数同理），不用记参数顺序
4. 第一个对于传 falsy 的值无效，用函数参数的默认值


## Arrow Functions

```js
// bad
[1, 2, 3].map(function (x) {
	const y = x + 1;
	return x * y;
});

// good
[1, 2, 3].map((x) => {
	const y = x + 1;
	return x * y;
});

// ---------------------------

// bad
[1, 2, 3].map(number => {
	const nextNumber = number + 1;
	`A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// good
[1, 2, 3].map((number) => {
	const nextNumber = number + 1;
	return `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map((number, index) => ({
	[index]: number
}));
```

Note:
注意第二个例子的编码规范：如果只有一个参数并且后面是直接返回的内容（不是大括号括起来的代码块），参数就不加括号


## Class


#### Always use _class_ rather than manipulating _prototype_ directly.

```js
// bad
function Queue(contents = []) {
	this.queue = [...contents];
}
Queue.prototype.pop = function () {
	const value = this.queue[0];
	this.queue.splice(0, 1);
	return value;
};

// good
class Queue {
	constructor(contents = []) {
		this.queue = [...contents];
	}
	pop() {
		const value = this.queue[0];
		this.queue.splice(0, 1);
		return value;
	}
}
```

Note:
更简洁，原型链虽说要理解但是尽量少直接在上面操作从而避免出错（避免破坏原型链）


#### Use _extends_ for inheritance.

```js
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
	Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
	return this._queue[0];
}

// good
class PeekableQueue extends Queue {
	peek() {
		return this._queue[0];
	}
}
```



## 编程指导理念


## [Code First or Model First?](https://www.zhihu.com/question/28006283)

Note:
又是一个容易被忽视的关键性基础问题。

各有优缺点：

Code First：可以对数据模型做版本管理，代码里面就可以看到数据库数据模型的定义，可以做各种注释（重要！没有注释接手项目的人可能根本不知道数据库里面某个字段表示什么意思，对理解业务造成很大的阻碍）。缺点就是之后对数据模型就行修改很难反映到生产环境的数据库上，现有的用 Sequelize 基本上是手动去加的（其实我觉得可以做到：在 mysql 表 和 information_schema 表里对自己建的表做内省，跟现有定义对比处理再 Alter table，但是实现可能比较麻烦）。

Model First：先建数据表，再用建的表生成 DAL（Data Access Layer） 代码，适合已经存在的项目。缺点就比较多了，不好做版本管理（以及上文提到的字段注释的问题），自动生成 DAL 代码对如何扩展是一个挑战，而且每次改变表的定义要重新生成 DAL 代码并且手动覆盖原有代码，这就要求必须要求 DAL 里不包含任何业务相关的内容（手动覆盖容易出问题，一不小心可能把写好的业务覆盖了）。

现在更为普遍接受的做法是 Code First，就连最新的 .net core 的 EF Web Framework 都倾向于 Code First 的策略，但是我司的 .net 的技术栈的同事们还是在用代码生成器这种 Model First 的理念指导开发，我个人觉得是已经过时了的。

另外，这还不是代码生成器的主要问题，代码生成器没有良好的 API 文档，没有良好的版本管理，发版更新都是在群里说一声而已。有更好的官方的可能几十上百人在维护的 Web 框架不用，非得自己造一些更烂的轮子。

而且，对于程序员来说，技术（或者说 API）不通用不与社区接轨很有问题，别人要问你平时怎么用 .net 写项目，你说我用公司的代码生成器，相对底层的东西你全都不知道，只是调一调代码生成器的 API，互相在说些什么都不知道，两脸懵逼，根本聊不到一块去，技术得不到社区的认可。甚至可能连如何从零开始写一个 Web 应用都不知道。这样做得不到社区的正面反馈，我觉得很有问题！反正我个人是觉得我司的 .net 技术栈是有点固步自封甚至有点刻意排斥新技术的。


## Principles

- DRY - Don't Repeat Yourself
- OCP - Open Close Principle
- SoC - Separation of Concerns
- IoC - Inversion of Control
- CoC - Configuration over Convention

Note:
主要的一些原则，像 Unix 哲学中单一职责这些都知道的就不说了。

- 不要重复，不要写出意大利面条式的代码。
- 开闭原则：对扩展开放，对修改封闭。比如某个处理各种消息类型的函数，添加一种新的消息类型时不必修改主函数。解决的办法可以是给每个消息类型都传递自己特有的回调函数，或者用观察者模式彻底解耦都可以。关键在于解耦。
- 关注点分离：另一个方面的解耦，比如 MVC 模式、模板引擎分离过去 PHP 典型的 HTML 和数据、Parser 将数据和程序逻辑流程分离。
- 控制反转。核心思想就是杰哥在年会分享的 "Don't call me, I'll call you"（也叫好莱坞原则，好莱坞经纪人的口头禅）。回调最大的问题不在于 Callback Hell，而在于回调函数的调用没法得到有效控制。Promise 等各种异步流程管理库基本都是利用控制反转来使得程序流程能有效的控制在自己的手中的。
- 约定优于配置，也叫作按约定编程，它的意思是：为了简单起见，我们写代码按照一定的约定写（代码放在什么目录，用什么文件名，用什么类名等），这样省去了很多不必要的麻烦（但也不失flexibility，因为约定可以通过配置修改）。框架等偏底层的东西考虑的应该是多一些约束（这样团队协作中才能写出易读易理解的好代码），而第三方工具库、包这些东西则应该多一些兼容，接口应该考虑更多的调用方式。约束对编程有时候其实是一件好事，一个个人的不成熟的小想法：解释型语言相比编译型语言（脚本语言）缺少约束、而强类型约束严格（一般的解释型的都是弱类型），Python 将这两点中和得很好，所以约束得恰到好处，用起来就很受欢迎。


## Indirection / Layering
#### the most important guideline of software developing

Note:
不多说，直接进下一页


### All problems in computer science can be solved by another level of indirection.
#### —— David Wheeler

Note:
David Wheeler: 英国计算机科学家，世界上第一个获得计算机 PhD 学位的人，剑桥大学三一学院



## Thank You
#### [maples7.com](http://maples7.com)