> 原文地址 [docs.chatopera.com](https://docs.chatopera.com/products/chatbot-platform/integration/api.html)

[](#chatbot类)`Chatbot`类
-----------------------
## nodejs安装 
`npm install @chatopera/sdk --save`

### [](#实例化)实例化

`Chatbot`类是与 Chatopera 云服务集成的一个核心类，因为 Chatopera 云服务为开发者提供聊天机器人服务，`Chatbot`类的对象就是 Chatopera 云服务中一个聊天机器人的代理。

#### [](#构造函数)构造函数

```
Chatbot(clientId, secret [, serviceProvider])
```

#### [](#参数说明)参数说明

<table><thead><tr><th>name</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>clientId</td><td>string</td><td>✔</td><td>在<a href="https://bot.chatopera.com/dashboard" target="_blank">机器人控制台 / 机器人 / 设置</a>中获取</td></tr><tr><td>secret</td><td>string</td><td>✔</td><td>获取办法同上</td></tr><tr><td>serviceProvider</td><td>string</td><td>✘</td><td>Chatopera 机器人平台地址，<br>当使用 Chatopera 云服务时，该值为 https://bot.chatopera.com，也是默认值</td></tr></tbody></table>

#### [](#更多实例化例子)更多实例化例子

不同语言下，`Chatbot`类的包名或引用方式不同，Node.js SDK 的实例化上文已经表述，以下再介绍其它语言。

##### Java

```
import com.chatopera.bot.sdk.Chatbot;
...
Chatbot chatbot = new Chatbot(clientId, secret);
```

##### Python

```
from chatopera import Chatbot
bot = Chatbot(clientId, secret)
```

##### PHP

假设使用 [composer](https://getcomposer.org/) 作为包管理工具，其它安装方式参考 [chatopera-php-sdk](https://github.com/chatopera/chatopera-php-sdk)。

```
<?php

include_once **DIR** . "/vendor/autoload.php";
$chatbot = new Chatopera\SDK\Chatbot($appId, \$secret);

```

##### Go

```
import (
	"github.com/chatopera/chatopera-go-sdk"
)
...
var chatbot = chatopera.Chatbot(clientId, secret)
```

### [](#发送请求)发送请求

`Chatbot`实例的核心接口是`command`，以下也使用`Chatbot#command`来指这个接口，该接口是对 RestAPI Request 的高级封装，内部完成**签名认证**，**RequestHeaders** 和 **RequestBody** 等处理。

#### [](#接口规范)接口规范

```
result = chatbot.command(method, path [, body])
```

> **提示：** result 返回在 Node.js 中使用`await`或`Promise`，参考[快速开始](https://docs.chatopera.com/products/chatbot-platform/integration/quick-get-start.html)；其它语言直接用 `=` 便可获取。

#### [](#参数说明-1)参数说明

<table><thead><tr><th>name</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>method</td><td>string</td><td>✔</td><td>对于资源的具体操作类型，由 HTTP 动词表示。有效值包括<code>GET</code>，<code>POST</code>，<code>PUT</code>，<code>DELETE</code>和<code>HEAD</code>等</td></tr><tr><td>path</td><td>string</td><td>✔</td><td>资源的执行路径，通常包含资源实体名称或唯一标识，也可能在 <code>path</code>中使用<code>queryString</code>传递参数</td></tr><tr><td>body</td><td><code>JSON</code>数据结构</td><td>❓</td><td><code>body</code> 是请求中的数据，对应 RestAPI 中的 Http Body</td></tr></tbody></table>

`method`不同动词代表的含义一般如下：

*   GET - 从服务器取出一项或多项资源；
*   POST - 在服务器创建一个资源；
*   PUT - 在服务器更新一个资源；
*   DELETE - 在服务器删除一个资源。

还有更多类型的`method`，没有上述几种常用，在此不进行赘述。

`queryString`是 URL 的一部分。典型的 URL 看起来像这样: http://server/resource?foo=A&bar=B。其中，foo=A&bar=B 就是`queryString`，通常用来传递参数，这个例子中包含两个参数：`foo`值为`A`；`bar`值为`B`。在下文中，`path`参数中可能包含`queryString`，形式如 foo={{var1}}&bar={{var2}}，需要把`{{var1}}`和`{{var2}}`替换为实际值。

`body`数据是 JSON 格式的，不同语言对于 JSON 格式支持方式不同。[JSON](https://www.json.org/json-en.html) 是一种轻量级的数据交换格式，描述了使用键值对、数组、字符串、数字、日期和布尔类型等值存储对象。[JSON](https://www.json.org/json-en.html) 在不同语言下，等价数据结构如下。

<table><thead><tr><th>语言</th><th>JSON Object</th><th>JSON Array</th></tr></thead><tbody><tr><td>JavaScript</td><td><code>{...}</code></td><td><code>[...]</code></td></tr><tr><td>Java</td><td><a href="https://www.tutorialspoint.com/org_json/org_json_jsonobject.htm" target="_blank">org.json.JSONObject</a></td><td><a href="https://www.tutorialspoint.com/org_json/org_json_jsonobject.htm" target="_blank">org.json.JSONArray</a></td></tr><tr><td>PHP</td><td>基本类型<code>array</code></td><td>基本类型<code>array</code></td></tr><tr><td>Python</td><td>基本类型<code>dict</code></td><td>基本类型<code>list</code></td></tr><tr><td>Go</td><td><code>map[string]interface{}</code></td><td><code>[]map[string]interface{}</code></td></tr></tbody></table>

**下文表述时，统一使用`JSON`，`JSON Object`和`JSON Array`代表 JSON 数据结构和其不同语言下的等价数据结构。**

> **提示：** 相对而言，JSON 等价的数据结构，在获取`JSON Object`的键值或`JSON Array`的长度和成员时，语法不同，但都易于掌握。在使用时，参考不同 SDK 的[示例程序](https://docs.chatopera.com/products/chatbot-platform/integration/index.html#%E4%B8%8B%E8%BD%BD-sdk)。

**`body`是否必填以及是`JSON Object`还是`JSON Array`，取决于`method`和`path`的值，不同`method`和`path`的组合对应了不同的接口功能，满足不同需求，下文将介绍满足各种需求的`method`和`path`，并各个说明`body`参数。**

### [](#返回值)返回值

**返回值**即请求结果，针对接口定义，`Chatbot#command`的返回值`result`是 `JSON Object`，并有以下属性。

<table><thead><tr><th>key</th><th>type</th><th>description</th></tr></thead><tbody><tr><td><code>rc</code></td><td>int</td><td>response code，返回码，大于等于 0 的正整型。<code>0</code>代表服务器端按照请求描述，正常返回结果；<code>rc</code> 不等于 0 是代表异常返回。</td></tr><tr><td><code>data</code></td><td>JSON</td><td>数据资源。正常返回时，服务器端执行逻辑成功，比如查询时，<code>data</code>就是查询结果。</td></tr><tr><td><code>msg</code></td><td>string</td><td>消息，当服务器端执行请求成功，并且不需要返回数据资源时，通过 <code>msg</code>代表文本信息，比如提示信息。</td></tr><tr><td><code>error</code></td><td>string</td><td>异常消息，当服务器端返回异常时，具体出错信息包含在<code>error</code>中。</td></tr><tr><td><code>status</code></td><td>JSON</td><td>全局任务的状态信息。</td></tr><tr><td><code>total</code></td><td>int</td><td>分页，所有数据记录条数。</td></tr><tr><td><code>current_page</code></td><td>int</td><td>分页，当前页码，（分页从 1 开始）。</td></tr><tr><td><code>total_page</code></td><td>int</td><td>分页，所有页数。</td></tr></tbody></table>

每次请求结果中，`rc`是必含有的属性，其它属性为可能含有。不同`rc`的正整数形代表不同的异常，`data`、`status`以及分页信息，则因`method`和`path`而异，以下进行详细介绍。

> **提示：** 不同语言对返回值可能进行了封装，但是不离其宗，都是基于以上定义，比如 Java SDK 中，定义`com.chatopera.bot.sdk.Response`作为`Chatbot#command`接口返回值，`Response`类提供`getRc`、`getData`和`toJSON`等方法，提升代码可读性。在使用时，参考不同 SDK 的[示例程序](https://docs.chatopera.com/products/chatbot-platform/integration/index.html#%E4%B8%8B%E8%BD%BD-sdk)。

下文中使用的`method`，`path`，`body`和`result`等均代表以上介绍的概念。

[](#机器人画像)机器人画像
---------------

### [](#获取机器人画像)获取机器人画像

```
Chatbot#command("GET", "/")
```

#### [](#result-json-object)result / JSON Object

```
{
    "rc": 0,
    "data": {
        "name": "小巴巴",
        "fallback": "请联系客服。",
        "description": "Performs Tasks or retrieves FAQ.",
        "welcome": "你好，我是机器人小巴巴",
        "primaryLanguage": "zh_CN",
        "status": {
            "reindex": 0,
            "retrain": 0
        }
    }
}
```

<table><thead><tr><th>key</th><th>type</th><th>description</th></tr></thead><tbody><tr><td><code>name</code></td><td>string</td><td>机器人名字。</td></tr><tr><td><code>fallback</code></td><td>string</td><td>兜底回复，当请求机器人对话时，没有得到来自多轮对话、知识库或意图识别回复时，回复此内容。</td></tr><tr><td><code>welcome</code></td><td>string</td><td>机器人问候语。</td></tr><tr><td><code>description</code></td><td>string</td><td>机器人描述。</td></tr><tr><td><code>primaryLanguage</code></td><td>string</td><td>机器人语言。</td></tr><tr><td><code>status</code></td><td>JSON Object</td><td>全局任务的执行状态，<code>reindex</code>代表知识库同步自定义词典的状态；<code>retrain</code>代表意图识别同步自定义词典的状态。</td></tr></tbody></table>

### [](#更新机器人画像)更新机器人画像

```
Chatbot#command("PUT", "/", body)
```

#### [](#body-json-object)body / JSON Object

```
{
 "fallback": "请联系客服。",
 "description": "我的超级能力是对话",
 "welcome": "你好，我是机器人小巴巴"
}
```

#### [](#result-json-object-1)result / JSON Object

```
{
 "rc": 0,
 "data": {
  "name": "小巴巴",
  "fallback": "请联系客服。",
  "description": "Performs Tasks or retrieves FAQ.",
  "welcome": "你好，我是机器人小巴巴"
}
```

### [](#获取全局任务状态)获取全局任务状态

```
Chatbot#command("GET", "/status")
```

#### [](#result-json-object-2)result / JSON Object

```
{
 "rc": 0,
 "data": {
  "status": {
   "reindex": 0,
   "retrain": 0
  }
}
```

[](#对话检索)对话检索
-------------

### [](#检索知识库)检索知识库

```
Chatbot#command("POST", "/faq/query", body)
```

#### [](#body-json-object-1)body / JSON Object

```
{
	"query": "查找相似的问题",
	"fromUserId": "{{userId}}",
	"faqBestReplyThreshold": 0.5,
	"faqSuggReplyThreshold": 0.1
}
```

#### [](#result-json-object-3)result/ JSON Object

```
{
    "rc": 0,
    "data": [
        {
            "id": "{{docId}}",
            "score": 0.48534,
            "post": "查看相似问题不可能的",
            "replies": [
                {
                    "rtype": "plain",
                    "enabled": true,
                    "content": "方法"
                }
            ]
        },
        {
            "id": "{{docId}}",
            "score": 0.32699,
            "post": "聊天",
            "replies": [
                {
                    "rtype": "plain",
                    "content": "foo",
                    "enabled": true
                },
                {
                    "rtype": "plain",
                    "content": "bar",
                    "enabled": true
                }
            ]
        }
    ]
}
```

### [](#检索意图识别)检索意图识别

意图识别是基于请求者的文本内容分析意图，然后基于意图追问意图槽位信息的对话，这部分的详细介绍参考 [https://docs.chatopera.com/products/chatbot-platform/intent.html](https://docs.chatopera.com/products/chatbot-platform/intent.html)，下面重点介绍在系统集成中，通过意图识别服务提供智能问答。

#### [](#什么是会话)什么是 “会话”

“会话”(session) 在代表一个用户对话的周期，认为用户在这个周期内是为了完成某个任务的。从确定任务，到得到和这个任务相关的信息，这个 session 就正常结束了，但是如果用户变化了任务，这个 session 就不能正常结束。开发者选择什么时候创建新的 session，但是服务器端决定什么时候完成这个 session，session 的管理涉及：意图的确定，意图参数的确定，会话最大空闲时间，会话是否解决 (resolved)。

*   训练完成后请求对话，需要先创建会话，会话会绑定 0-1 个任务：刚开始不知道用户意图，当确定用户意图后，该 session 就只和这个意图相关；
*   会话有最大空闲日期，如果在半个小时内没有更新，会被服务器删除；
*   会话可以任意创建，只要没有超过最大空闲日期都是有效的；
*   不同的用户使用不同的会话，同一个用户可以同时有多个会话，但是为了实际效果，用户最好同时只使用一个会话；
*   当用户的意图和槽位信息被全部确认，会话包含的 resolved 字段会被设置为 true，这时开发者可以再次创建一个新的会话。

#### [](#创建会话)创建会话

```
Chatbot#command("POST", "/clause/prover/session", body)
```

#### [](#body-json-object-2)body / JSON Object

```
{
	"uid": "{{userId}}",
	"channel": "{{channelId}}"
}
```

<table><thead><tr><th>key</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>userID</td><td>string</td><td>✔</td><td>用户标识，由字母和数字组成的字符串。开发者自定义，保证每个用户唯一</td></tr><tr><td>channelId</td><td>string</td><td>✔</td><td>用户来源的渠道标识，由字母和数字组成的字符串。由开发者自定义，保证每个渠道唯一</td></tr></tbody></table>

#### [](#result-json-object-4)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "intent_name": null,
        "uid": "{{userId}}",
        "channel": "{{channelId}}",
        "resolved": null,
        "id": "{{sessionId}}",
        "entities": null,
        "createdate": "2019-08-28 18:08:51",
        "updatedate": "2019-08-28 18:08:51"
        "ttl": 3600
    },
    "error": null
}
```

_intent_name_: 意图名字

_id_: 会话 ID

_resolved_: 该会话是否完成收集参数

_entities_: 参数列表，完成填槽或待填槽

_ttl_: 该会话信息在多少秒后过期，每个会话默认是 1 小时的空闲周期，在该时间内没有跟进的对话，则会话过期

#### [](#检索意图识别-1)检索意图识别

```
Chatbot#command("POST", "/clause/prover/chat", body)
```

#### [](#body-json-object-3)body / JSON Object

```
{
	"fromUserId": "{{userId}}",
	"session": {
		"id": "{{sessionId}}"
	},
	"message": {
		"textMessage": "我想购买明天火车票"
	}
}
```

<table><thead><tr><th>key</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>userId</td><td>string</td><td>✔</td><td>用户唯一 ID，用户 ID 由业务系统传递或生成，保证每个用户用唯一字符串</td></tr><tr><td>sessionId</td><td>string</td><td>✔</td><td>使用创建会话接口创建</td></tr><tr><td>textMessage</td><td>string</td><td>✔</td><td>用户输入的对话文字</td></tr></tbody></table>

#### [](#result-json-object-5)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "session": {
            "intent_name": "{{intentName}}",
            "uid": "{{userId}}",
            "channel": "{{channelId}}",
            "resolved": false,
            "id": "{{sessionId}}",
            "entities": [
                {
                    "name": "cityName",
                    "val": "中国首都"
                }
            ],
            "createdate": "2019-08-28 18:15:24",
            "updatedate": "2019-08-28 18:15:24",
             "ttl": 3595
        },
        "message": {
            "textMessage": "你想做什么工具",
            "is_fallback": null,
            "is_proactive": true
        }
    },
    "error": null
}
```

#### [](#查看会话详情)查看会话详情

```
Chatbot#command("GET", "/clause/prover/session/{{sessionId}}")
```

#### [](#result-json-object-6)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "intent_name": "{{intentName}}",
        "uid": "{{userId}}",
        "channel": "{{channelId}}",
        "resolved": false,
        "id": "{{sessionId}}",
        "entities": null,
        "createdate": "2019-08-28 18:41:56",
        "updatedate": "2019-08-28 18:41:56",
        "ttl": 3600
    },
    "error": null
}
```

### [](#检索多轮对话)检索多轮对话

多轮对话是通过脚本规则、函数编程实现问答服务，在_检索多轮对话_接口中，同时融合了知识库参与回复决策，返回结果，尤其是通过知识库答案路由到指定话题的指定触发器，非常实用。

```
Chatbot#command("POST", "/conversation/query", body)
```

#### [](#body-json-object-4)body / JSON Object

```
{
    "fromUserId": "{{userId}}",
    "textMessage": "想要说些什么",
    "faqBestReplyThreshold": 0.6,
    "faqSuggReplyThreshold": 0.35
}
```

<table><thead><tr><th>key</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>userId</td><td>string</td><td>✔</td><td>用户唯一 ID，用户 ID 由业务系统传递或生成，保证每个用户用唯一字符串</td></tr><tr><td>textMessage</td><td>string</td><td>✔</td><td>用户输入的对话文字</td></tr><tr><td>faqBestReplyThreshold</td><td>number</td><td>✘</td><td>知识库最佳回复， 默认 0.8，知识库建议回复，知识库中置信度超过该值通过返回值<code>string</code>和<code>params</code>返回</td></tr><tr><td>faqSuggReplyThreshold</td><td>number</td><td>✘</td><td>知识库建议回复，默认 0.6，知识库中置信度超过该值的问答对通过返回值<code>faq</code>属性返回</td></tr></tbody></table>

#### [](#result-json-object-7)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "state": "default",
        "string": "方法",
        "logic_is_unexpected": false,
        "logic_is_fallback": false,
        "service": {
            "provider": "faq",
            "docId": "{{doctId}}",
            "score": 0.3781,
            "threshold": 0.37
        },
        "botName": "小巴巴",
        "faq": [
            {
                "id": "{{doctId}}",
                "score": 0.3781,
                "post": "查看相似问题不可能的",
                "replies": [
                    {
                        "rtype": "plain",
                        "enabled": true,
                        "content": "方法"
                    }
                ]
            }
        ]
    }
}
```

_state_: 业务字段，可以在多轮对话脚本中设置

_string_: 机器人回复的文本内容

_topicName_: 机器人会话主题

_logic_is_fallback_: 是否是兜底回复

_botName_: 机器人的名字

_faq_: 知识库中匹配 textMessage 的相似度超过 **faqSuggReplyThreshold** 的记录，数组类型

`service`代表返回的数据来源，**provider:script** 指**多轮对话**，**provider:faq** 指**知识库**；不同数据来源也会提供相应信息。

<table><thead><tr><th>provider</th><th>key</th><th>解释</th></tr></thead><tbody><tr><td>faq</td><td></td><td></td></tr><tr><td></td><td>docId</td><td>文档 ID</td></tr><tr><td></td><td>post</td><td>标准问</td></tr><tr><td></td><td>score</td><td>分数</td></tr><tr><td></td><td>threshold</td><td>阀值</td></tr><tr><td>conversation</td><td>多轮对话</td><td></td></tr><tr><td>fallback</td><td>兜底回复</td><td></td></tr><tr><td>mute</td><td>该用户被该机器人屏蔽</td><td></td></tr></tbody></table>

#### [](#服务器端逻辑)服务器端逻辑

多轮对话获取回复的逻辑解释如下：

<table><caption>查询逻辑</caption><tbody><tr><td><img class="" src="https://docs.chatopera.com/images/products/chatbot-engine-1.png"></td></tr></tbody></table>

1.  用户输入，以文本的形式输入，语音输入也需要转化成文字。
2.  [知识库检索] 如果知识库检索出相似度大于 `faqBestReplyThreshold` 的问答对，直接返回得分最高的问题的答案。
3.  [多轮对话检索] 如果知识库没有检索出相似度大于 `faqBestReplyThreshold` 的问答对，检索多轮对话，如果命中了一个规则，直接返回答案。
4.  [兜底回复] 如果多轮对话也没有检索出答案，返回兜底回复。

#### [](#知识库路由)知识库路由

在[知识库的答案](https://docs.chatopera.com/products/chatbot-platform/faq/qna.html#%E8%AE%BE%E5%AE%9A%E7%AD%94%E6%A1%88)或[多轮对话的函数](https://docs.chatopera.com/products/chatbot-platform/conversation/function.html)中设置回复时，可以用 **routeDirectReply** 来检索一个指定的[话题](https://docs.chatopera.com/products/chatbot-platform/conversation/index.html#%E6%9C%AF%E8%AF%AD)和[匹配器](https://docs.chatopera.com/products/chatbot-platform/conversation/index.html#%E5%8C%B9%E9%85%8D%E5%99%A8)。

语法：

```
routeDirectReply#["TOPIC_NAME", "TOPIC_GAMBIT_ID" [,INHERIT_PARAMS]]
```

_TOPIC_NAME_: [对话名称](https://docs.chatopera.com/products/chatbot-platform/conversation/index.html#%E6%9C%AF%E8%AF%AD)

_TOPIC_GAMBIT_ID_: [触发器名称](https://docs.chatopera.com/products/chatbot-platform/conversation/index.html#%E6%9C%AF%E8%AF%AD)

其中，_INHERIT_PARAMS_ 是可选参数，决定当前对话取得的 `params` 是否覆盖接下来对话的 `params`，值为`[true|false]`，默认为 `false`。

另外，当 `TOPIC_GAMBIT_ID` 的值为 `$ctx.textMessage$` 时，则使用当前对话的用户输入，在 `TOPIC_NAME` 中进行检索。

比如

```
routeDirectReply#["class_001_pre", "__C1PRE_GAMBIT_003",true]
```

<table><caption>【知识库或函数】路由多轮对话</caption><tbody><tr><td><img class="" src="https://docs.chatopera.com/images/products/platform/set-faq-route-conversion-reply.jpg"></td></tr></tbody></table>

提示：**routeDirectReply** 需要设定为知识库问答对里的第一个答案，答案类型为 纯文本`plain`。

[](#词典管理)词典管理
-------------

### [](#创建自定义词典)创建自定义词典

```
Chatbot#command("POST", "/clause/customdicts", body)
```

#### [](#body-json-object-5)body / JSON Object

```
{
	"name": "{{customDictName}}",
	"type": "vocab"
}
```

<table><thead><tr><th>key</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>name</td><td>string</td><td>✔</td><td>自定词典名称，使用<code>小写字母和数据</code>组成的字符串</td></tr></tbody></table>

#### [](#result-json-object-8)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "name": "{{customDictName}}",
        "description": "",
        "samples": null,
        "createdate": "2019-08-07 19:59:14",
        "updatedate": "2019-08-07 19:59:14"
    }
}
```

### [](#获取自定义词典列表)获取自定义词典列表

```
Chatbot#command("GET", "/clause/customdicts?limit={{limit}}&page={{page}}")
```

#### [](#result-json-object-9)result/ JSON Object

```
{
    "rc": 0,
    "total": 3,
    "current_page": 1,
    "total_page": 3,
    "data": [
        {
            "name": "{{customDictName}}",
            "description": "",
            "samples": null,
            "createdate": "2019-08-07 19:58:08",
            "updatedate": "2019-08-07 19:58:08"
        }
    ]
}
```

### [](#更新自定义词典)更新自定义词典

```
Chatbot#command("PUT", "/clause/customdicts/{{customDictName}}", body)
```

#### [](#path)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>customDictName</td><td>string</td><td>无默认值, 必填</td><td>自定义词典标识</td></tr></tbody></table>

#### [](#body-json-object-6)body / JSON Object

```
{
    "description": "高级轿车品牌"
}
```

#### [](#result-json-object-10)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "name": "pizza",
        "description": "",
        "samples": null,
        "createdate": "2020-07-20 20:52:00",
        "updatedate": "2020-07-20 20:51:59",
        "type": "vocab",
    }
}
```

### [](#删除自定义词典)删除自定义词典

```
Chatbot#command("DELETE", "/clause/customdicts/{{customDictName}}")
```

#### [](#path-1)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>customDictName</td><td>string</td><td>无默认值, 必填</td><td>自定义词典标识</td></tr></tbody></table>

#### [](#result-json-object-11)result/ JSON Object

```
{
    "rc": 0,
    "msg": "success",
    "error": null,
    "data": {
        "status": {
            "needReindex": 2,
            "needRetrain": 2
        }
    }
}
```

[](#知识库管理)知识库管理
---------------

### [](#创建知识库分类)创建知识库分类

```
Chatbot#command("POST", "/faq/categories", body)
```

#### [](#body-json-object-7)body / JSON Object

```
{
	"label": "{{categoryText}}"
}
```

#### [](#result-json-object-12)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "value": "{{categoryId}}",
        "categories": [
            {
                "value": "{{categoryId}}",
                "label": "{{categoryText}}",
                "children": [
                    {
                        "value": "I7vfx47i5I",
                        "label": "二级分类名"
                    }
                ]
            },
            {
                "value": "{{categoryId}}",
                "label": "x2"
            }
        ]
    }
}
```

### [](#获取知识库分类信息)获取知识库分类信息

```
Chatbot#command("GET", "/faq/categories")
```

#### [](#body-json-object-8)body / JSON Object

```
{
    "rc": 0,
    "data": [
        {
            "value": "{{categoryId}}",
            "label": "{{categoryText}}",
            "children": [
                {
                    "value": "{{categoryId}}",
                    "label": "{{categoryText}}"
                }
            ]
        }
    ]
}
```

### [](#更新知识库分类)更新知识库分类

```
Chatbot#command("", "/faq/categories", body)
```

#### [](#body-json-object-9)body / JSON Object

```
{
	"value": "{{categoryId}}",
	"label": "新的名字"
}
```

#### [](#result-json-object-13)result/ JSON Object

```
{
    "rc": 0,
    "data": [
        {
            "value": "wwQyjS310",
            "label": "一级分类名",
            "children": [
                {
                    "value": "{{categoryId}}",
                    "label": "新的名字"
                }
            ]
        }
    ]
}
```

### [](#删除知识库分类)删除知识库分类

```
Chatbot#command("DELETE", "/faq/categories/{{categoryId}}")
```

#### [](#path-2)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>categoryId</td><td>string</td><td>无默认值，必填</td><td>分类唯一标识</td></tr></tbody></table>

#### [](#result-json-object-14)result/ JSON Object

```
{
    "rc": 0,
    "data": [
        {
            "value": "TSDD-W6T9",
            "label": "x2"
        }
    ]
}
```

### [](#创建问答对)创建问答对

```
Chatbot#command("post", "/faq/database", body)
```

#### [](#body-json-object-10)body / JSON Object

```
{
 "post": "如何查看快递单号",
 "replies": [
  {
   "rtype": "plain",
   "content": "foo",
   "enabled": true
  },
  {
   "rtype": "plain",
   "content": "bar",
   "enabled": true
  }
 ],
 "enabled": true,
 "categoryTexts": [
  "一级分类名",
  "二级分类名"
 ]
}
```

#### [](#result-json-object-15)result / JSON Object

```
{
 "rc": 0,
 "data": {
  "id": "{docId}",
  "replyLastUpdate": "{{replyLastUpdate}}"
 }
}
```

### [](#更新知识库问答对)更新知识库问答对

```
Chatbot#command("PUT", "/faq/database/{{docId}}", body)
```

#### [](#path-3)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>docId</td><td>string</td><td>无默认值, 必填</td><td>问答对标识</td></tr></tbody></table>

#### [](#body-json-object-11)body / JSON Object

```
{
	"post": "怎么开通微信支付?",
	"replyLastUpdate": "{{replyLastUpdate}}",
	"replies": [
		{
			"rtype": "plain",
			"content": "foo2",
			"enabled": true
		},
		{
			"rtype": "plain",
			"content": "bar2",
			"enabled": true
		}
	],
	"enabled": true
}
```

#### [](#result-json-object-16)result / JSON Object

```
{
    "rc": 0,
    "data": {
        "id": "{{docId}}",
        "replyLastUpdate": "{{replyLastUpdate}}"
    }
}
```

### [](#获取问答对列表)获取问答对列表

```
Chatbot#command("GET", "/faq/database?limit={{limit}}&page={{page}}&q={{q}}")
```

#### [](#path-4)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>limit</td><td>int</td><td>1</td><td>返回最多多少条数据</td></tr><tr><td>page</td><td>int</td><td>20</td><td>返回第多少页</td></tr><tr><td>q</td><td>string</td><td>空</td><td>问答对匹配时，问题应包含的关键字</td></tr></tbody></table>

#### [](#result-json-object-17)result / JSON Object

```
{
    "total": 3,
    "current_page": 1,
    "total_page": 1,
    "data": [
        {
            "post": "如何查看快递单号",
            "categories": [
                "wwQyjS310",
                "I7vfx47i5I"
            ],
            "enabled": true,
            "id": "{{docId}}"
        }
    ],
    "rc": 0,
    "status": {
        "reindex": 0,
        "retrain": 0
    }
}
```

### [](#创建问答对相似问)创建问答对相似问

```
Chatbot#command("POST", "/faq/database/{{docId}}/extend", body)
```

#### [](#path-5)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>docId</td><td>string</td><td>无默认值, 必填</td><td>问答对标识</td></tr></tbody></table>

#### [](#body-json-object-12)body / JSON Object

```
{
	"post": "怎样支持微信支付?"
}
```

#### [](#result-json-object-18)result / JSON Object

```
{
    "rc": 0,
    "data": {
        "id": "{{extendId}}"
    }
}
```

### [](#获取问答对相似问列表)获取问答对相似问列表

```
Chatbot#command("GET", "/faq/database/{{docId}}/extend")
```

#### [](#path-6)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>docId</td><td>string</td><td>无默认值, 必填</td><td>问答对标识</td></tr></tbody></table>

#### [](#result-json-object-19)result / JSON Object

```
{
    "total": 1,
    "current_page": 1,
    "total_page": 1,
    "data": [
        {
            "post": "怎样支持微信支付?",
            "postId": "{{docId}}",
            "enabled": true,
            "id": "{{extendId}}"
        }
    ],
    "rc": 0
}
```

### [](#更新问答对相似问)更新问答对相似问

```
Chatbot#command("PUT", "/faq/database/{{docId}}/extend/{{extendId}}", body)
```

#### [](#path-7)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>docId</td><td>string</td><td>无默认值, 必填</td><td>问答对标识</td></tr><tr><td>extendId</td><td>string</td><td>无默认值, 必填</td><td>扩展问标识</td></tr></tbody></table>

#### [](#body-json-object-13)body / JSON Object

```
{
	"post": "怎样支持微信支付?"
}
```

#### [](#result-json-object-20)result / JSON Object

```
{
    "rc": 0,
    "data": {
        "id": "{{extendId}}"
    }
}
```

### [](#删除问答对相似问)删除问答对相似问

```
Chatbot#command("DELETE", "/faq/database/{{docId}}/extend/{{extendId}}")
```

#### [](#path-8)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>docId</td><td>string</td><td>无默认值, 必填</td><td>问答对标识</td></tr><tr><td>extendId</td><td>string</td><td>无默认值, 必填</td><td>扩展问标识</td></tr></tbody></table>

#### [](#result-json-object-21)result / JSON Object

```
{
    "rc": 0,
    "msg": "done"
}
```

### [](#删除问答对)删除问答对

```
Chatbot#command("DELETE", "/faq/database/{{docId}}")
```

#### [](#result-json-object-22)result / JSON Object

```
{
    "rc": 0,
    "msg": "done"
}
```

[](#语音识别)语音识别
-------------

目前，Chatopera 机器人平台只支持中文简体 （zh_CN）机器人做中文语音识别。

语音文件格式：16k 采样率，单通道，PCM。

```
Channels       : 1
Sample Rate    : 16000
Precision      : 16-bit
Sample Encoding: 16-bit Signed Integer PCM
```

下载[音频示例](https://docs.chatopera.com/images/products/platform/asr.sample.001.wav)。

语音识别接口可使用两种形式提交语音文件：1）文件路径；2）语音文件 base64 格式字符串。

### [](#提交文件路径识别)提交文件路径识别

```
Chatbot#command("POST", "/asr/recognize", body)
```

#### [](#body-json-object-14)body / JSON Object

```
{
	"filepath": "{{WAV_FILE_ABS_PATH}}",
	"nbest": 5,
	"pos": true
}
```

<table><thead><tr><th>key</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>filepath</td><td>string</td><td>✔</td><td>语音文件绝对路径，或当前应用启动路径 (CWD) 的相对路径</td></tr><tr><td>nbest</td><td>int</td><td>✘</td><td>语音识别可返回多个结果，方便查询关键词，默认 5</td></tr><tr><td>pos</td><td>boolean</td><td>✘</td><td>返回值是否分词，默认 false</td></tr></tbody></table>

#### [](#result-json-object-23)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "duration": 6250,
        "predicts": [
            {
                "confidence": 0.960783,
                "text": "上海浦东机场入境房输入全闭环管理"
            },
            {
                "confidence": 0.960767,
                "text": "上海浦东机场入境防输入全闭环管理"
            },
            {
                "confidence": 0.960736,
                "text": "上海浦东机场入境坊输入全闭环管理"
            }
        ]
    }
}
```

<table><thead><tr><th>key</th><th>type</th><th>description</th></tr></thead><tbody><tr><td><code>duration</code></td><td>int</td><td>语音文件时间长度，单位 毫秒，比如 6250 代表 6.25 秒</td></tr><tr><td><code>predicts</code></td><td>JSONArray</td><td>识别结果</td></tr><tr><td><code>text</code></td><td>string</td><td>识别得到的文本</td></tr><tr><td><code>confidence</code></td><td>double</td><td>置信度，[0-1]，值越大越有可能，<code>predicts</code> 按 <code>confidence</code> 降序</td></tr></tbody></table>

### [](#提交-base64-字符串识别)提交 base64 字符串识别

```
Chatbot#command("POST", "/asr/recognize", body)
```

#### [](#body-json-object-15)body / JSON Object

```
{
	"type": "base64",
	"data": "data:audio/wav;base64,{{BASE64_STRING}}",
	"nbest": 5,
	"pos": true
}
```

<table><thead><tr><th>key</th><th>type</th><th>required</th><th>description</th></tr></thead><tbody><tr><td>type</td><td>string</td><td>✔</td><td>固定值 <code>base64</code></td></tr><tr><td>data</td><td>string</td><td>✔</td><td>语音文件使用 base 编码的字符串，并且必须以 <code>data:audio/wav;base64,</code>作为前缀，比如 <code>data:audio/wav;base64,xyz...</code></td></tr><tr><td>nbest</td><td>int</td><td>✘</td><td>语音识别可返回多个结果，方便查询关键词，默认 5</td></tr><tr><td>pos</td><td>boolean</td><td>✘</td><td>返回值是否分词，默认 false</td></tr></tbody></table>

此处，`nbest`和`pos` 与 **提交文件路径识别** API 一致。

#### [](#result-json-object-24)result/ JSON Object

返回值与 **提交文件路径识别** API 一致。

```
{
    "rc": 0,
    "data": {
        "duration": 6250,
        "predicts": [
            {
                "confidence": 0.960783,
                "text": "上海浦东机场入境房输入全闭环管理"
            },
            {
                "confidence": 0.960767,
                "text": "上海浦东机场入境防输入全闭环管理"
            },
            {
                "confidence": 0.960736,
                "text": "上海浦东机场入境坊输入全闭环管理"
            }
        ]
    }
}
```

<table><thead><tr><th>key</th><th>type</th><th>description</th></tr></thead><tbody><tr><td><code>duration</code></td><td>int</td><td>语音文件时间长度，单位 毫秒，比如 6250 代表 6.25 秒</td></tr><tr><td><code>predicts</code></td><td>JSONArray</td><td>识别结果</td></tr><tr><td><code>text</code></td><td>string</td><td>识别得到的文本</td></tr><tr><td><code>confidence</code></td><td>double</td><td>置信度，[0-1]，值越大越有可能，<code>predicts</code> 按 <code>confidence</code> 降序</td></tr></tbody></table>

[](#用户管理)用户管理
-------------

### [](#获取用户列表)获取用户列表

```
Chatbot#command("GET", "/users")
```

#### [](#path-9)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>limit</td><td>int</td><td>1</td><td>返回最多多少条数据</td></tr><tr><td>page</td><td>int</td><td>20</td><td>返回第多少页</td></tr></tbody></table>

#### [](#result-json-object-25)result / JSON Object

```
{
    "rc": 0,
    "total": 5,
    "current_page": 1,
    "total_page": 1,
    "data": [
        {
            "userId": "{{userId}}",
            "lasttime": "2020-07-19T14:12:13.690Z",
            "created": "2020-07-19T13:48:02.225Z"
        }
    ]
}
```

_userId_: 和机器人对话的用户标识

_lasttime_: 最后沟通时间

_created_: 第一次沟通时间

### [](#屏蔽用户)屏蔽用户

```
Chatbot#command("POST", "/users/{{userId}}/mute")
```

#### [](#path-10)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>userId</td><td>string</td><td>无默认值, 必填</td><td>用户唯一标识</td></tr></tbody></table>

#### [](#result-json-object-26)result / JSON Object

```
{
    "rc": 0,
    "data": {}
}
```

### [](#取消屏蔽)取消屏蔽

```
Chatbot#command("POST", "/users/{{userId}}/unmute")
```

#### [](#path-11)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>userId</td><td>string</td><td>无默认值, 必填</td><td>用户唯一标识</td></tr></tbody></table>

#### [](#result-json-object-27)result / JSON Object

```
{
    "rc": 0,
    "data": {}
}
```

### [](#是否被屏蔽)是否被屏蔽

```
Chatbot#command("POST", "/users/{{userId}}/ismute")
```

#### [](#result-json-object-28)result / JSON Object

```
{
    "rc": 0,
    "data": {
        "mute": false
    }
}
```

`data.mute`返回 boolean 类型值。

### [](#获取用户画像信息)获取用户画像信息

```
Chatbot#command("GET", "/users/{{userId}}/profile")
```

#### [](#path-12)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>userId</td><td>string</td><td>无默认值, 必填</td><td>用户唯一标识</td></tr></tbody></table>

#### [](#result-json-object-29)result/ JSON Object

```
{
    "rc": 0,
    "data": {
        "userId": "postman9",
        "name": null,
        "lasttime": "2020-07-19T14:12:13.690Z",
        "created": "2020-07-19T13:48:02.225Z",
        "profile": {},
        "mute": false
    }
}
```

### [](#获取聊天历史)获取聊天历史

```
Chatbot#command("GET", "/users/{{userId}}/chats?limit={{limit}}&page={{page}}")
```

#### [](#path-13)path

<table><thead><tr><th>key</th><th>type</th><th>default</th><th>description</th></tr></thead><tbody><tr><td>userId</td><td>string</td><td>无默认值, 必填</td><td>用户唯一标识</td></tr><tr><td>limit</td><td>int</td><td>1</td><td>返回最多多少条数据</td></tr><tr><td>page</td><td>int</td><td>20</td><td>返回第多少页</td></tr></tbody></table>

#### [](#result-json-object-30)result / JSON Object

```
{
    "rc": 0,
    "total": 16,
    "current_page": 1,
    "total_page": 1,
    "data": [
        {
            "userId": "postman9",
            "textMessage": "方法",
            "direction": "outbound",
            "service": "faq",
            "confidence": 0.3781,
            "docId": "AXNCspoufXOJhfysI3-Z",
            "created": "2020-07-19T14:12:13.802Z"
        }
    ]
}
```

_total_: 该用户和机器人之间对话总数

_current_page_： 当前页

_total_page_: 总页数

_userId_: 用户标识

_textMessage_: 文本内容

_direction_: 消息传递方向，【inbound】为消费者发送，【outbound】为机器人发送

_service_: 提供回复的服务

_confidence_: 置信度

_created_: 消息创建时间

[](#评论)评论
---------
