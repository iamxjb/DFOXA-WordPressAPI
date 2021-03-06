DFOXA-WordPressAPI
====================
WordPress API 扩展插件,允许使用 WordPres 开发标准的API接口,为基于 WordPress 的前后端分离项目实现便捷轻快的后端开发体验.


> 接口文档会在 1-3个月内完善,英文文档会在中文文档完善后的1个月内发布,插件用户请按照组内教程或组内提问.<b>不要提交 issues </b>
> Interface documents will be improved within 1-3 months, the English document will be released within 1 month after the improvement of the Chinese document and plug-in users could ask questions in the group.

## 了解 DFOXA-WordPressAPI


>DFOXA 项目是为了快速开发基于 WordPress API接口所开发的 WordPress 插件。
>它适用于所有的前后端分离项目,例如使用 [Vue.js](https://vuejs.org)、AngularJS、[Electron](http://electron.atom.io)、[微信小程序](https://mp.weixin.qq.com/debug/wxadoc/introduction/)、支付宝小程序等框架开发的项目。
>DFOXA 提供了 WordPress 的所有基本功能接口,包括用户授权、注册、登录、文章、分类、评论等...。
>DFOXA 额外提供了一些优秀的功能,例如可使用SMS接口实现用户账号的注册,登录的短信验证码等功能。
>DFOXA 提供的插件模式,弥补了接口的的不足,你可以基于 基础功能 或 WordPress（PHP） 的所有能力独立开发 API接口,例如 开发商城系统 等...。
>DFOXA 为您解决了数据交互的安全性问题(使用RSA加密),以及跨域、数据缓存、日志记录问题。
>DFOXA 配置使用了最新的 Composer ,你可以通过 Composer 进行丰富的功能扩展。

## 准备工作
* 如果您的项目是一个完全独立的前后端分离项目,
    或您的项目完全用不到WordPress自带的主题功能(不需要前端展示主题),
    建议您移除WordPress的主题功能,只访问WordPress后台进行开发.
 
 ```php
 // 要完全移除 主题功能 请将 WordPress 程序根目录下的 index.php 文件中的
 define('WP_USE_THEMES', true);
 // 改为
 define('WP_USE_THEMES', false);
 ```
* 为你的接口配置一个合格的域名,例如(api.domain.com) 本文档所有用到的示例域名,都将使用 api.domain.com 作为演示域名,请留意
* 在微信小程序等相关项目开发时,你需要配置 [HTTPS](https://www.vpser.net/build/letsencrypt-certbot.html)
* 固定链接,虽然可能你不需要对外展示你的 WordPress 主题相关内容,但是为了接口的正常使用,你还是得配置一个合适的固定链接,对于固定链接的格式并没有做要求,只要不是默认的<b>?p=123</b>即可,推荐使用的是<b>/%post_id%.html</b>
* 登录注册等功能需要你配置 PHP 的 OpenSSL 环境
* 登录注册等功能需要你配置 WordPress 自带的内存缓存功能 (测试生效方式 登录账号后验证access_token是否能通过,通过则表示内存缓存生效)

## 简单设置
> 安装完插件后,您需要在后台先配置你的插件,配置的步骤非常容易。

#### 缓存系统设置

DFOXA 暂时只支持 WordPress 自带缓存 ,它基于 <code>Memcache（d）或 Redis Object Cache</code>
你必须为他提供一个"[持久性](https://codex.wordpress.org/Class_Reference/WP_Object_Cache#Persistent_Caching)[?](https://wordpress.stackexchange.com/questions/48643/how-wp-cache-is-supposed-to-work-and-does-it-help-with-performance)"的缓存。
你必须配置 Memcache（d）,并安装相关的 WordPress 内存缓存插件,因为接口将大量使用缓存系统,如果你不做这一步,你将无法继续使用该插件

#### 日志记录

DFOXA 配置了日志记录插件 [Monolog](https://github.com/Seldaek/monolog),你需要在后台启用它,启用后你必须将插件目录下的 <b>logs</b> 目录配置对应的读写权限,他会自动记录所有的用户请求信息和相关错误信息,便于你的调试和问题排查。

## 接口调试

你的接口必须经过详细调试后才能发布于线上,我们推荐您使用下列工具进行调试,并使用Chrome的相关json格式化插件

工具: [POSTMAN](https://www.getpostman.com/apps)、[PAW](https://paw.cloud)、[JSONViewer 插件](https://github.com/tulios/json-viewer)

## 准备完毕

访问你的接口测试地址,开始你的第一步

https://api.domain.com/gateway.do?method=Tests.dfoxaState

* gateway.do 是您在后台设置页面配置的 API 网关,所有的接口都通过它进行调用,所以你的其他页面不会受到接口影响。
* 小窍门: 网关请求地址还有另外一种格式,去除 ?method= 并将接口内容中的 . 改为 /

> https://api.domain.com/gateway.do/Tests/dfoxaState</br>
> 同等于</br>
> https://api.domain.com/gateway.do?method=Tests.dfoxaState

* 直接通过浏览器访问你的这个接口,你会看到一条 JSON 格式的 返回值,他的内容如下

```json
{
  "code": 10000,
  "msg": "接口调用成功",
  "sub_msg": "接口调用成功",
  "solution": "",
  "sub_code": "10000",
  "hello": "看起来这个接口已经准备就绪了.",
  "request": null
}

// 错误网关的请求示例

{
  "code": 10002,
  "msg": "错误的请求地址",
  "sub_msg": "当前的接口请求地址有误,请检查后再试",
  "solution": "重新检查你的请求地址是否有误",
  "sub_code": "gateway.empty-method",
  "request": null
}
```
> 所有的接口都会返回类似格式的信息,</br>
> <b>code:</b></br>当接口的 code === 10000 表示这个接口已经调用成功,在使用任意接口的时候,你应当使用 code 值 是否等于 10000 来判断这个接口是否执行成功。</br>
> <b>msg / submsg:</b></br>接口的提示信息,如果接口请求错误,它们会返回请求错误的相关信息, msg 稍短一些 ,submsg 则更加详细一些,这些信息适合暴露给用户,例如账号密码错误等信息。</br>
> <b>solution:</b></br>接口错误的解决办法,通常这个信息不暴露给用户,在你调试的时候使用,返回一些接口错误时的解决方案。</br>
> <b>sub_code:</b></br>这个接口返回一些字符串格式的错误编号,和 code 是一一对应的。例如 <code>10002</code> 对应 <code>gateway.empty-method</code></br>
> <b>request:</b></br>这个接口返回一些用户的请求内容,方便前端进行开发调试。

## 接口文档

### 初级使用

* 插件的基本配置
* 账号
* 文章
* 分类
* 评论

### 进阶使用

> 为你的接口开发独一无二的功能（例如日历查询、商城系统、客资报名系统、电影查询、交通查询等...）,它的开发方式非常标准和简单,就像是开发 WordPress 插件一样,如果你开发过,那么它对你将是一件轻松的事情。</br>
> 你甚至可以在网络上自由的分享你的插件。

* 插件 Hooks
* 用户角色和权限
* 插件的开发


## Author

&copy; 2016 DooFox,Inc. - hoythan@gmail.com - https://doofox.cn</br>
See also the list of [contributors](https://github.com/hoythan/DFOXA-WordPressAPI/blob/master/md/contributors.md) which participated in this project.

## License

DFOXA-WordPressAPI is licensed under the MIT License - see the <code>LICENSE</code> file for details