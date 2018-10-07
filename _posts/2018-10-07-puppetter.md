---
layout: default
title:  Puppeteer 简介
---

<h1>{{ page.title }}</h1>

## 背景
在编写爬虫程序的时候免不了遇到网页重定向的问题，重定向方式可大概分为以下三种：
* 服务器端重定向,如响应代码301、302等。
* meta refresh。
* js 重定向。  

其中前两种都能很容易地获取到重定向的url，但第三种方式只能通过加载js代码的方式来获取。

## Puppeteer 概述
Puppeteer 是一个 Node 库，它提供了一个高级 API 来通过 DevTools 协议控制 Chromium 或 Chrome。  
Puppeteer API 是分层次的，反映了浏览器结构。  
*注意：在下面的图表中，浅色框体内容目前不在 Puppeteer 中体现。*  
![](../_pictures/puppeteer_achrive.png)
* Puppeteer 使用 DevTools 协议 与浏览器进行通信。  
* Browser 实例可以拥有浏览器上下文。  
* BrowserContext 实例定义了一个浏览会话并可拥有多个页面。  
* Page 至少有一个框架：主框架。 可能还有其他框架由 iframe 或 框架标签 创建。  
* frame 至少有一个执行上下文 - 默认的执行上下文 - 框架的 JavaScript 被执行。 一个框架可能有额外的与 扩展 关联的执行上下文。  
* Worker 具有单一执行上下文，并且便于与 WebWorkers 进行交互。  

## 安装
*puppeteer 运行与 Node v6.4.0以上，由于async/await在v7.6.0 之后才支持，因此建议使用Node v7.6.0以上版本*  
使用npm即可完成puppeteer的安装   
```shell
npm i puppeteer
```
使用该命令安装puppeteer会默认下载最新的Chromeium，如想改变这一行为，可以通过以下环境变量来实现：  
* HTTP_PROXY, HTTPS_PROXY, NO_PROXY - 定义用于下载和运行 Chromium 的 HTTP 代理设置。
* PUPPETEER_SKIP_CHROMIUM_DOWNLOAD - 请勿在安装步骤中下载绑定的 Chromium。
* PUPPETEER_DOWNLOAD_HOST - 覆盖用于下载 Chromium 的 URL 的主机部分。
* PUPPETEER_CHROMIUM_REVISION - 在安装步骤中指定一个你喜欢 puppeteer 使用的特定版本的 Chromium。

## 使用示例
### 获取截图
```javascript
const puppeteer = require('puppeteer');

(async () => {
  // 打开一个浏览器
  const browser = await puppeteer.launch();
  // 打开一个新页面
  const page = await browser.newPage();
  // 跳转到 https://example.com
  await page.goto('https://example.com');
  // 保存当前页面截图
  await page.screenshot({path: 'example.png'});


  // 关闭浏览器
  await browser.close();
})();
```

### 事件响应
在Node.js当中,很多对象都会触发事件，puppeteer正式通过这种方式将信息传递到处理函数当中，使得我们能够很简单地获取到需要的内容或者是对整个流程进行监视和控制。
```javascript
const puppeteer = require('puppeteer');
userAgent = '';

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  // 设置Header User-Agent
  await page.setUserAgent(userAgent)
  // 是否激活request.abort, request.continue 和 request.respond 等方法，一个请求之后可能会有多个异步请求，如加载图片等，通过该方法可以中断加载图片等行为
  await page.setRequestInterceptionEnable(true);
  await page.goto('https://example.com');
  
  // 当页面的某个请求接收到对应的 [response] 时触发。
  page.on('response', resp => {
      // response action
  });

  // 当页面发送一个请求时触发。参数 [request] 对象是只读的。 如果需要拦截并且改变请求,可将setRequestInterceptionEnable设为 true
  page.on('request', req => {
      // setRequestInterceptionEnable 为 true 时必须要调用 req.continue 才会进行请求
      req.continue();
  });

  // 当js对话框出现的时候触发。
  page.on('dialog', dialog => {
      // dialog action
  })

  // 当页面崩溃时触发。
  page.on('error', err => {
      // error action
  });

  // 当 iframe 加载的时候触发。
  page.on('frameattached', frm => {
      // frame action
  })

  // 当页面的 load 事件被触发时触发。
  page.on('load', () => {
      // laod action
  })

  await browser.close();
})();
```

### 修改页面源码
有时我们需要对页面源码进行一定的改动再重新执行，puppeteer同样也具备了这样的能力。
```javascript
const puppeteer = require('puppeteer');
url = 'https://example.com';

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  // 将 id 为 offer 的 element 的 src 的值改为 url 的值并重新打开该页面
  await page.evaluate(url => {
      document.getElementById("offer").src = url;
    }, url);

  await browser.close();
})();
```

在实际的使用当中，通过设置时间响应函数是获取数据的主要方式，结合修改页面源码与获取截图，可完成绝大多数爬虫所需要的功能。

### API
见[Puppeteer文档](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/?id=%e6%a6%82%e8%bf%b0https://zhaoqize.github.io/puppeteer-api-zh_CN/#/?id=puppeteer-%e4%b8%ad%e6%96%87%e6%96%87%e6%a1%a3)

#### 参考：
* [Puppeteer文档](https://zhaoqize.github.io/puppeteer-api-zh_CN/#/?id=%e6%a6%82%e8%bf%b0https://zhaoqize.github.io/puppeteer-api-zh_CN/#/?id=puppeteer-%e4%b8%ad%e6%96%87%e6%96%87%e6%a1%a3)

<p>{{ page.date | date_to_string }}</p>
