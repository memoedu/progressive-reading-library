## 前言

登录注册，渲染页面，增删改查，这些都是一个网站所必备的组件模块，它们足够基础，以至于你独立开发任何网站时，总会遇到。今天，我需要更进一步，为了让我的另一个商业作品可以盈利，我们需要引入支付功能。

国内合法的、长期的、手续费低的、安全可靠的支付工具其实就两个，微信支付和支付宝，很凑巧的是，它们也为开发者提供了便捷的支付工具包。

> 大家需要先形成一个基本概念——为了避免洗钱，逃税漏税等恶劣行为，支付行为本身都需要经历大量繁琐的审批环节的，而且还需要考虑到支付的安全性和可靠性等等，所以网络支付这件事本身是很难很难的，所以请不要在配置你的支付宝支付代码时抱怨为什么流程又臭又长。因为实际上比这更长。你日常使用的一个小小而支付按钮或者支付的二维码背后都是无数篇安全领域和后端开发的结晶。

本篇教程，就从资源指南的角度，记录一下如何使用支付宝的 sdk 来为我们的网站完成支付功能，这是咱们的小店能赚钱的第一步！

---

## 工具准备 & 烦人的前置工作

本篇教程采用 NextJS 作为前后端全栈框架，需要 nodejs 运行时和基础的 nextjs 脚手架，不会的自己网上查怎么安装。先让我们安装支付宝的 sdk:

```bash
npm install alipay-sdk
```

> 需要注意的是，「支付API」分为小程序，App和电脑端支付三种，本文主要讲解的是电脑端的办理和开发流程。

为了合法进行经营，我们还需要注册一家公司并申请一张公司的银行卡，并在 [支付宝｜商家平台](https://b.alipay.com/) 注册成为商家，具体你进入这个网站就知道了，旁边有引导的AI客服。

然后进入「产品中心」「我有电脑网站」，此处会显示当前实体是否通过审核，没有的话你需要提交一些当前公司的材料通过审核，这样，基于当前商家账号创建的支付工具才可用，否则用户扫描你创建出的收款二维码，用户的手机会显示「该商家收款额度已暂调整为0，请联系商家补全资料提升收款额度。」的字样。

![图片](https://kirigaya.cn/api/storage/image/bfa309b6-2993-4235-856e-45d9994b8cbb.webp)

为了避免洗钱和逃税漏税，支付宝的默认「电脑网站」端的支付交易额度每天都有上线，等你的小店经营范围扩大后，再来这个地方申请提升交易额度吧！

---

## 创建支付应用

当你有了支付宝商家账号后，就可以前往 [支付宝｜开放平台](https://open.alipay.com/develop/manage) 完成应用的创建，点击「网页/移动应用」创建一个新的支付应用。点击创建好的应用，就能进入如下的一个页面。

![图片](https://kirigaya.cn/api/storage/image/28b136cd-316f-4a01-b99a-c1cfd715ae07.webp)

为了让我们的后端可以安全可靠地接入当前的页面，我们需要点击「开发设置」，来配置一些选项。其中，最重要的就是第一条「接口加密方式」，我们需要先安装支付宝密钥工具，在 [密钥工具](https://opendocs.alipay.com/common/02kipk) 文档页面可以找到安装包和使用教程。根据教程在本地生成「应用公钥」和「应用私钥」，并将「应用公钥」上传到开放平台的加密配置页面，最终，你能看到生成的「支付宝公钥」。

到此，我们获得了能够激活支付功能的几个重要变量，打开我们的项目，在配置文件中写入：

```
ALIPAY_APP_ID={应用ID}
ALIPAY_PUBLIC_KEY={支付宝公钥}
ALIPAY_PRIVATE_KEY={应用私钥}
ALIPAY_GATEWAY={支付宝网关地址}
```

有了这几个变量，我们就可以调用支付功能了。

---

## 支付的基本步骤

接下来就是让我们的应用可以完成支付了，这里有些搜索能力很强的小伙伴可能就会去 google 或者询问大模型了，这里一定要注意了！支付宝真正能让你用的当前应用配置下的API，都在刚刚那个配置密钥页面的「可调用产品」页面，这个页面里面会罗列你有权限用的 api，网上搜到的大概率并不准确。

### 创建支付订单

这里简单讲讲电脑端支付的原理，下图中的用户就是你的网站的用户，商家就是你的网站的前后端。

![图片](https://kirigaya.cn/api/storage/image/f7e04e5d-13b2-4ba4-88f6-d47318c711bc.webp)

当用户通过你的网站下单准备支付时，你的网站首先通过 [alipay.trade.page.pay](https://opendocs.alipay.com/open/59da99d0_alipay.trade.page.pay) 接口来创建订单，订单创建成功后，支付宝服务器会给我们的网站后端发送一段 html 代码，这串 html 代码在浏览器中会动态执行，最终渲染出一个二维码供用户扫码支付，整个过程绝对安全。这串 html 强烈推荐在 iframe 中渲染，并且你可以通过 [alipay.trade.page.pay](https://opendocs.alipay.com/open/59da99d0_alipay.trade.page.pay) 文档里面的 qr_pay_mode 改变渲染的UI样式。

一个基本的创建电脑网站订单的代码如下：

```typescript
import { AlipaySdk } from "alipay-sdk";

const alipaySdk = new AlipaySdk({
    appId: process.env.ALIPAY_APP_ID,
    privateKey: process.env.ALIPAY_PRIVATE_KEY,
    alipayPublicKey: process.env.ALIPAY_PUBLIC_KEY,
    gateway: process.env.ALIPAY_GATEWAY
});

const result = alipaySdk.pageExec('alipay.trade.page.pay', {
    bizContent: {
        // 创建订单的编号，你自己生成
        out_trade_no: '123456',
        // 支付金额
        total_amount: '10.0',
        // 支付订单的标题
        subject: '测试支付',
        // 产品编号，电脑网站统一用这个
        product_code: 'FAST_INSTANT_TRADE_PAY',
        // 二维码的渲染样式，看文档
        qr_pay_mode: '0',
    },
});

console.log(result);
```

输出结果为：

```html
<form name="punchout_form" method="post" action="https://openapi.alipay.com/gateway.do?charset=UTF-8&method=alipay.trade.page.pay&format=json&sign=ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE&version=1.0&app_id=2017060101317939&sign_type=RSA2&timestamp=2014-07-24+03%3A07%3A50">
<input type="hidden" name="biz_content" value="{&quot;time_expire&quot;:&quot;2016-12-31 10:05:01&quot;,&quot;extend_params&quot;:&quot;&quot;,&quot;query_options&quot;:&quot;[\&quot;hyb_amount\&quot;,\&quot;enterprise_pay_info\&quot;]&quot;,&quot;settle_info&quot;:&quot;&quot;,&quot;subject&quot;:&quot;Iphone6 16G&quot;,&quot;product_code&quot;:&quot;FAST_INSTANT_TRADE_PAY&quot;,&quot;body&quot;:&quot;Iphone6 16G&quot;,&quot;qr_pay_mode&quot;:&quot;1&quot;,&quot;integration_type&quot;:&quot;PCWEB&quot;,&quot;merchant_order_no&quot;:&quot;20161008001&quot;,&quot;sub_merchant&quot;:&quot;&quot;,&quot;invoice_info&quot;:&quot;&quot;,&quot;ext_user_info&quot;:&quot;&quot;,&quot;timeout_express&quot;:&quot;90m&quot;,&quot;disable_pay_channels&quot;:&quot;pcredit,moneyFund,debitCardExpress&quot;,&quot;agreement_sign_params&quot;:&quot;&quot;,&quot;royalty_info&quot;:&quot;&quot;,&quot;store_id&quot;:&quot;NJ_001&quot;,&quot;request_from_url&quot;:&quot;https://&quot;,&quot;qrcode_width&quot;:&quot;100&quot;,&quot;goods_detail&quot;:&quot;&quot;,&quot;enable_pay_channels&quot;:&quot;pcredit,moneyFund,debitCardExpress&quot;,&quot;out_trade_no&quot;:&quot;20150320010101001&quot;,&quot;total_amount&quot;:&quot;88.88&quot;,&quot;business_params&quot;:&quot;{\&quot;mc_create_trade_ip\&quot;:\&quot;127.0.0.1\&quot;}&quot;,&quot;promo_params&quot;:&quot;{\&quot;storeIdType\&quot;:\&quot;1\&quot;}&quot;}">
<input type="submit" value="立即支付" style="display:none" >
</form>
<script>document.forms[0].submit();</script>
```

这段代码放入浏览器会将整个 DOM 渲染为下面这样的图片：

![图片](https://kirigaya.cn/api/storage/image/ad7928ab-953e-48ae-b7f9-021235401743.webp)

把它插入我们的网站的前端的支付组件的 iframe 中即可。

### 2. 等待用户完成/取消支付更新数据库

然后后续用户完成或者取消支付的流程都会通过支付宝自己的服务器完成，如果我们希望记录这一过程并录入我们自己的数据库中，从而**可靠地**确认用户是否完成了某个订单的支付有两个方法：

1. 在实例化支付客户端 `alipaySdk` 时，再传入一个 notify_url 回调请求。那么当支付订单被创建后，每一个关键的事件（支付成功，支付失败等等）支付宝服务器都会往 notify_url 这个链接去发送请求，而我们在 notify_url 路由下挂载一个函数，就可以知道用户什么时候支付完成了。

2. 创建支付订单后，我们通过轮询 [alipay.trade.query](https://opendocs.alipay.com/open/bff76748_alipay.trade.query) 接口来获取订单状态，从而知晓当前的支付结果，然后更新数据库。获取订单的编号 tradeNo 也就是我们上文中自己创建的编号 123456。

无论哪种方法，前端部分都需要轮询后端的数据库来不断获取支付结果，一旦成功后，就显示支付成功并刷新前端的各个相关变量的状态。

一个查询的代码如下：

```typescript
const alipaySdk = new AlipaySdk({
    appId: process.env.ALIPAY_APP_ID,
    privateKey: process.env.ALIPAY_PRIVATE_KEY,
    alipayPublicKey: process.env.ALIPAY_PUBLIC_KEY,
    gateway: process.env.ALIPAY_GATEWAY
});

const response = await alipaySdk.exec('alipay.trade.query', {
    bizContent: {
        out_trade_no: '123456'
    }
});

const tradeStatus = response?.tradeStatus;
```

tradeStatus 定义如下：

```typescript
export type TradeStatus = 'WAIT_BUYER_PAY' | 'TRADE_CLOSED' | 'TRADE_SUCCESS' | 'TRADE_FINISHED';
```

一般情况下只要是 TRADE_SUCCESS 或者 TRADE_FINISHED 就说明支付成功了。

> 需要说明的是，如果你设置了白名单，那么上面的代码只有在白名单运行的IP上运行才有效。似乎只有创建订单功能没有这个限制。

---

## 结语

和登录注册类似，支付系统也是一个成熟的商业网站必须具备的东西，祝愿诸位早日也能做出自己的商业化作品吧！只有经历市场验证的技术才是真正厉害的，有价值的技术。支付则是让你的「技术」蜕变为「产品」的必由之路，这样的路还有很多，一起加油吧！

诸位，和气生财呀。