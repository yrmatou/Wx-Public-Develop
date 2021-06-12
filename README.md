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
URL和token校验通过后可以直接开发你的需求业务代码  
![image](https://user-images.githubusercontent.com/21699695/121777720-2171c780-cbc6-11eb-9a67-3f49a025fd14.png)  
建议是先在微信测试平台开发测试 开发测试完后再配置到自己的公众号上面  

### 服务器运行
* pm2 守护进程
