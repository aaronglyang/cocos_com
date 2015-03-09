  本文档除有特别说明外，数据传输都使用get方式发送，返回数据一律使用json格式。接口返回的 json 数据会根据业务情况进行增加，客户端不应假定json 数据中返回的对象数据个数。
Oauth2.0 授权流程图

![image](img/oauth.png)


目前仅实现并提供站内应用接入、登录接入、订单创建等相关功能，暂未上线Oauth2.0 登录授权页功能。以上流程图中
Client ： 即本文档所称的第三方开发者
Resource Owner : 即平台的注册用户
Authorization Server 和 Resource Server 目前均指开发者平台，即这些服务都由开发者平台服务器提供服务。
返回数据中的各种key名称，除非特别说明，默认均为小写。
以下接口中相关实现都基于默认已登录并授权访问。
###签名规则（通用）
签名规则适应用于所有需要进行签名验证的接口。
在请求参数列表中，除去sign参数外，其他需要使用到的参数都是要签名的参数。
对参数按a-z的顺序进行排序，然后使用&符号，将需要签名的数据按照key=val的形式连接起来最后附加上app_secret的值，示例URL：
`https://passport.cocos.com/oauth/token/?client_id=103&app_key=aeb09dcb8e1eab0d1306625b268d5e2a&grant_type=password&password=111111&username=hhhhhh@chukong-inc.com&sign=1f04f8520ce4808761aa4fc1ad04e838`
该URL生成的签名字符串前面部分为：`app_key=aeb09dcb8e1eab0d1306625b268d5e2a&client_id=103&grant_type=password&password=111111&username=hhhhhh@chukong-inc.com`


假设其app_secret为：
`090efb8c3d3a6107b59202f765f18343`那么查询字符串连接app_secret后生成的字符串为：
`app_key=aeb09dcb8e1eab0d1306625b268d5e2a&client_id=103&grant_type=password&password=111111&username=hhhhhh@chukong-inc.com090efb8c3d3a6107b59202f765f18343`通过md5后生成最终的签名字符串为：
`1f04f8520ce4808761aa4fc1ad04e838`
注意：
__1. 关于签名字符串，最后生成的应用都是小写。__
__2. 待签名的字符串值应当是没有进行过URL Ecodeing的原值，而且传输的过种中，如果参数中含有特殊字符，如@、&之类的，在传输的时候，需要进行URL Ecodeing操作。__
__3. 生成签名字符串必须是所有除sign本身以外的其它所有参数，即使其值为空。__

###站内应用接入


示例：http://cn.cocos.com/demo/play?app_id=613934525&app_key=4e62a8e22db0fe0a5e2db487ba4282a9&app_platform=1&create=1412818164&expire=1412904564&session=849a0a464212d4b624e0de7b52d1c943&uid=400053&sign=5c64367fb7d181a7a592c3521da6322f
(注：以上地址目录不可访问，仅做URL展示示例)

传入第三方应用的参数

| 参数名称 | 是否必须 | 参数解释说明|
|————|———-|———-|
|app_id|必须|开发者的应用ID|


