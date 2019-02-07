#  ssrc
ssrc提供了多人在线编辑代码的网站

## 实现功能
以下为实现功能列表，加*者为基础功能，优先处理

-  友好实用、简洁的UI*
- 支持用户注册、登录*
-  支持代码高亮*
- 支持多种编程语言
- 支持在线白板演示
-  支持在线聊天功能

## Contributors wanted
下面是通过评估后预计需要的人手，其中*者为必备，每个人承担至少一项任务(以模块分配)

- 前端开发人员若干*
- 后端开发人员若干*
- 文档编写
- 测试

## 开发流程
本项目采用PR模式进行多人协作开发， 开发者首先fork源项目，进行本地开发， 然后通过github中的Pull Request进行合并请求，由项目管理者labsl进行审核并合并~~其实本体是trisolaris233~~

## 接口设计

为了实现前后端尽量解耦，需要统一接口进行设计。

其中参数格式一律为`json`， 方便设计。

尽量使用`AJAX`实现数据的收发。

### 基本数据结构

**User**
```json
{
	"username" : "trisolaris233",
	"password" : "abcdefg",
	"avatarpath" : "/image/1.png",
	"id":1
}
```
基础用户拥有三个字段, `username`, `password`,`avatarpath`, 分别为用户名，密码、头像路径和id

**要求**：

用户名长度限制：`40`字符长度(一个汉字也算作1)

密码长度限制：`18`字符长度

`id`为整数从1开始自然增长

**Room**

```json
{
	"name" : "python&chat",
	"password" : "python666",
	"source_type" : 3,
	"id":1
}
```

`Room`拥有四个字段，`name`, `password`和`source_type`, `id`分别为房间名、密码、源代码类型和房间号

**要求**:

房间名长度限制： `20`字符长度

密码长度限制：`12`字符长度

`source_type`表示源码类型

```C++
enum source_type {
	C_ = 1,
	Cpp = 2,
	Python = 3
}
```
`id`为整数从1开始自然增长

----------------

### 基本请求和响应方式
基本请求方式为`GET`,`POST`，当请求方式为`POST`时一律使用`json`数据
每次请求服务端**必须**响应。

#### 请求格式
请求格式以设计的基本数据结构为准， 有特殊时会说明。
#### 响应格式
响应格式基本结构如下:
```json
{
	"state": "1或者0",
	"code": "具体状态码",
	"msg": "说明内容",
	"data": {
		按照具体请求的东西来
	}
}
```
因此,`state`, `code`,`msg`和`data`字段必须存在， 内容是具体情况而定。

**一些例子**:

#### 用户注册

请求方式:  `POST`, `GET`

请求地址: `/register`

参数:

```json
{
	"usename": "用户名",
	"password": "密文密码, sha512",
	"vcode": "验证码"
}
```

响应:

```json
{
	"state" : "",
	"code":"",
	"msg":"",
	"data": {
		空
	}
}
```


请求方式：`POST`, `GET`
请求地址: `/signin`
参数：
```json
{
 	"username": "用户名",
 	"salt": "1-65536的随机数盐",
 	"password": "sha512(sha512(password)+salt)"
}
```

响应:
```json
{
	"state":"",
	"code":"",
	"msg":"",
	"data":{
		空
	}
}
```


#### 获取用户信息
请求方式： `GET`， `POST`
请求地址： 	`/(用户名)`
参数:
```json
{
	"username" : "用户名"
}
```

响应:
```json
{
	"state": "",
	"code": "",
	"msg":"",
	"data": {
		"username": "",
		"avatarpath":"",
		"id":""
	}
}
``` 

-----

#### 创建房间
请求方式：`GET`, `POST`
请求地址：`/createroom`
参数:
```json
{
	"name": "房间名",
	"password": "房间密码",
	"source_type": 语言类型
}
```

## 模块化
