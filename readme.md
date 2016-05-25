# 知乎数据 API 接口 (node.js)

> 知乎已经更新为 https, 本项目 \< 1.0.0 不能再使用了. 请升级

[![][image-1]][1]

[![][image-2]][2]

根据这些接口获取到知乎的数据，包括以下接口：
* [用户 User API][3]
* [专栏文章 Post API][4]
* [答案 Answer API][5]
* [问题 Question API][6]
* [话题 Topic API][7]

**欢迎贡献代码，一起完善知乎的接口**

## 快速开始

	var zhihu = require('zhihu');

## demo
获取用户的信息

	var username = 'shanejs';
	zhihu.User.getUserByName(name).then(function(user){
	  console.log(user);
	});

结果

	{ answer: 14,
	 post: 0,
	 follower: 529,
	 profileUrl: 'https://www.zhihu.com/people/shanejs',
	 name: '狂飙蜗牛',
	 sex: 'male' }

## 用户 User API
### User.getUserByName(username)
根据用户名获取到用户的信息，用户名为用的唯一标识，参见个人主页的url，或者设置中的个性网站  

* `username`  //用户位置标识

**Example** 
 
请求这个用户：[https://www.zhihu.com/people/shanelau1021][8]  
`name` 为 `shanelau1021`

	var nam = 'shanelau1021';
	zhihu.User.getUserByName(name).then(function(user){
	  console.log(user);
	});

**Result**

参数说明

* `answer`答题数量
* `post` 文章数量
* `follower` 跟随者数量
* `profileUrl` 个人主页
* `name` 名字
* `sex`性别

	{ answer: 5,
	 post: 0,
	 follower: 456,
	 profileUrl: 'https://www.zhihu.com/people/shanelau1021',
	 name: '狂飙蜗牛',
	 sex: 'male' }


## 专栏文章 Post API
### Post.info(postUrl)
获取专栏文章的详细信息

* `postUrl`  文章的url地址

**Example**

	zhihu.Post.info(postUrl).then(function(data){
	  //do something
	});

**Result**  

* Object

[example][9]

### Post.page(name,options)
获取专栏文章列表

* `name` 专栏的英文名字， 例如：'bigertech'
* `options`  {object}  ,默认值为10

	 {
	   limit: 10   // 记录数
	   offset: 10  // 偏移量
	 }


**Example**

[demo][10]


### Post.likersDetail(postUrl)
获取文章的点赞者的详细信息

* `postUrl`  文章的url地址

**Result**  

用户数组

* `{Array}`   //User


### Post.zhuanlanInfo(name)
获取专栏的信息

* `name`  专栏的名字，比如 `bigertech`

**Result**  

	{ followersCount: 22614,
	 description: '',
	 creator:
	 { bio: '魅族营销中心招募设计师',
	 hash: '29c3654588fd4246bb90cbd345242d65',
	 description: '',
	 profileUrl: 'http://www.zhihu.com/people/linan',
	 avatar:
	 { id: '24f3a654b',
	 template: 'http://pic2.zhimg.com/{id}\_{size}.jpg' },
	 slug: 'linan',
	 name: '李楠' },
	 topics: \[],
	 href: '/api/columns/bigertech',
	 acceptSubmission: true,
	 slug: 'bigertech',
	 name: '笔戈科技',
	 url: '/bigertech',
	 avatar:
	 { id: 'a4bf61d95',
	 template: 'http://pic3.zhimg.com/{id}\_{size}.jpg' },
	 commentPermission: 'anyone',
	 following: false,
	 postsCount: 173,
	 canPost: false,
	 activateAuthorRequested: false }

* `{Array}`   //User



## 答案 Answer API
### getVotersById(string answerId)
用 answerId 请求这个回答的点赞者。不同于 url 中显而易见的 "/question/numbers/answer/othernumbers/" 的数字，answerId 是相对隐含的。比如 "/question/28207685/answer/39974928"，这个回答的 answerId 应为 "11382008", 可以在 DOM tree 中找到。具体的对应关系正在探索。

@TODO 实现知乎支持的更多参数，比如 offset 等

## 问题 Question API
### focus
问题的关注列表  
@TODO

### Collection
问题的收藏列表
`url : http://www.zhihu.com/collection/25547043?page=1`


#### getAllPageData
获取所有的页面数据,遍历所有的页面

	Collection.getAllPageData(url);

#### getDataByPage,
获取某一页的页面数据

	var url = 'http://www.zhihu.com/collection/25547043?page=1';
	Collection.getDataByPage(url);

#### getPagination
获取改收藏列表的分页信息

	{
	 pages: 总页数，
	 current： 当前页面
	}


### info
问题的详细信息
@TODO



### questions
用户的提问列表  
@TODO

### answers
用户的回答列表  
@TODO
### zhuanlansFocus
用户关注的专栏  
@TODO

### topic
用户关注的话题信息  
@TODO



## 话题 Topic API

### Topic.getTopicByID(topicID[, page])
根据话题id获取该话题下的问题，话题id为唯一标识，参见话题的url
- `topicID` 话题的ID

**Example**  

请求这个话题：[生活、艺术、文化与活动][11]  
`topicID` 为 `19778317`


	var topicID = '19778317';
	    zhihu.Topic.getTopicByID(topicID).then(function(result){
	    console.log(result);
	});


  **Result**

  参数说明

  * `name` 话题名称
  * `page` 当前页数
  * `totalPage` 该话题下问题总页数
  * `questions` 当页问题
	  -    `title` 问题名字
	  -    `url` 问题链接
	  -    `postTime` 问题最近更新时间


	 { name: '生活、艺术、文化与活动',
	 page: 1,
	 totalPage: 47242,
	 questions:
	 { '0':
	 { title: '为什么很多人能接受有过长期恋爱经历，却不能接受有过婚姻的人？',
	 url: 'http://www.zhihu.com/question/27816723',
	 postTime: '41 秒前' },
	 '19':
	 { title: '360卫士在C盘为什么不可以删掉？',
	 url: 'http://www.zhihu.com/question/27816632',
	 postTime: '5 分钟前' } } }

## 贡献者
1. shanelau
2. Ivan Jiang (iplus26)

## 更新记录
#### 2016.5.23
1. 修复 https 问题
2. 修改部分bug
3. 加入 jscs 格式化代码风格

#### 2015.10.15
1. 新增收藏列表的数据抓取
2. 查询某个收藏下的所有数据和分页数据

[1]:	https://nodei.co/npm/zhihu/
[2]:	https://travis-ci.org/iplus26/zhihu/builds
[3]:	#%E7%94%A8%E6%88%B7-user-api
[4]:	#%E4%B8%93%E6%A0%8F%E6%96%87%E7%AB%A0-post-api
[5]:	#%E7%AD%94%E6%A1%88-answer-api
[6]:	#%E9%97%AE%E9%A2%98-question-api
[7]:	#%E8%AF%9D%E9%A2%98-topic-api
[8]:	http://www.zhihu.com/people/shanelau1021
[9]:	https://zhuanlan.zhihu.com/api/columns/bigertech/posts/19885136
[10]:	https://zhuanlan.zhihu.com/api/columns/bigertech/posts?limit=1&offset=10
[11]:	http://www.zhihu.com/topic/19778317/questions

[image-1]:	https://nodei.co/npm/zhihu.png?downloads=true "NPM"
[image-2]:	https://travis-ci.org/iplus26/zhihu.svg