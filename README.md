### Wx-Public-Develop
* 关于微信公众号开发相关-本地调试到线上运行 node koa  
* 方便快捷的本地开发调试  

### 首先配置开发环境
* [登录微信公众测试平台](https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login)  
* [公众号接口测试平台](https://mp.weixin.qq.com/debug/)  
* [frp 内网穿透工具准备](https://github.com/wangQiaoBrother/FrpUse)  

### 本地node koa
**首先配置接口URL校验**
```js
const Router = require('koa-router');
const sha1 = require('sha1');
const { baseUrl } = require('@config/config');
const router = new Router({
    prefix: baseUrl
});

// 验证token
router.get('/', async (ctx) => {
    // 暂时先不校验参数
    const {
        signature,
        timestamp,
        nonce,
        echostr
    } = ctx.query
    const { token } = global.config.wx
    // 进行字典排序加密
    let str = [token, timestamp, nonce].sort().join("");
    let sha = sha1(str)
    // 校验微信加密签名,返回echostr内容
    if (sha === signature) {
        ctx.body = echostr
    } else {
        ctx.body = "hello world"
    }
})
```
URL和token校验通过后可以直接开发你的需求业务代码，业务代码各有千秋    
![image](https://user-images.githubusercontent.com/21699695/121777720-2171c780-cbc6-11eb-9a67-3f49a025fd14.png)  
建议是先在微信测试平台开发测试 开发测试完后再配置到自己的公众号上面  
**夸一个微信做这个微信公众号测试平台真的很爽，本地用这个开发所有需求跟功能，开发完直接可以部署自己公众号**

### 服务器运行
* pm2 守护进程  

**强烈推荐福利！！！每天免费领取饿了么，美团外卖红包（买菜，点美食都可以）**

<div align="left">
  <img src="https://user-images.githubusercontent.com/21699695/140758752-182a4db2-ec40-4154-a090-4aba56583862.jpg" alt="Editor" width="200">
</div>

**互相学习交流**

<div align="left">
  <img src="https://user-images.githubusercontent.com/21699695/123603292-4f911180-d82c-11eb-809b-9c9f6232ba04.png" alt="Editor" width="200">
</div>
