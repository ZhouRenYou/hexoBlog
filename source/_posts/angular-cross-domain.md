---
title: angular开发环境与生产环境跨域处理
date: 2019-05-15 17:42:52
tags:
- 前端
- Angular
categories: Angular
---
# 跨域代理

## 前端

### JSON

1、根目录下新增proxy.config.json/proxy.config.js

```ts
// json 
{
  "/api": {
    "target": "service address",
    "secure": false,
    "logLevel": "debug",
    "changeOrigin": true,
    "pathRewrite": {
      "^/apidata": ""
    }

  }
}

```
```js
// js
const PROXY_CONFIG = [{
  context: [
    "/api"
  ],
  target: "http://xxxx:8764/", //测试环境
  target: "http://xx:8764",  //测试环境
  secure: false
}]

module.exports = PROXY_CONFIG;

```

2、angular.json配置文件加载配置代理文件proxyconfig.json/proxy.config.js

```ts
"serve": {
  "builder": "@angular-devkit/build-angular:dev-server",
  "options": {
    "browserTarget": "project1:build",
      "proxyConfig": "proxy.config.json"/"proxyConfig": "proxy.config.js"
  }
```
<!--more-->
3、component使用 

```ts
login(params: { userCode: any; password: any; }): Observable<any> {
    return this.http.post('api/account/login', params);
  }
```

4、重启运行

```ts
ng serve --proxy-config proxy.config.json
```
## Nginx（前后端部署在不同的服务器）

#### nginx.conf server节点下新增

``` conf
    location /api {
            proxy_pass   http://xxx.xx.x.xx:8766; # 后端接口 IP:port 跨域代理
        }

```

# 参考

## [Proxy To Backend](https://github.com/angular/angular-cli/blob/master/docs/documentation/stories/proxy.md)