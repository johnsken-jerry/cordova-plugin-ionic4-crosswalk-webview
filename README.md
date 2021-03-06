# 前言

该插件的功能是在`cordova ionic4`项目中使用`cordova-plugin-crosswalk-webview`浏览器内核。


## 介绍

使用了`cordova-plugin-crosswalk-webview`作为 cordova 浏览器内核，cordova使用`file:///`协议访问app内置的html页面，但由于`angular` `ionic4`中无法兼容`file:///` 协议。

因此在ionic4中，ionic官方提供了一个插件`cordova-plugin-ionic-webview`，将`file:///`协议替换成了`http://`协议，以兼容`angular`框架，但是该插件调用的`android`系统自带的浏览器，且无法同时兼容`cordova-plugin-crosswalk-webview`插件。

为了能在`ionic4`项目中同时使用`cordova-plugin-crosswalk-webview`,可以安装此插件。

## 安装

使用前需卸载`cordova-plugin-ionic-webview`与`cordova-plugin-crosswalk-webview`

``` shell
cordova plugin remove cordova-plugin-crosswalk-webview
cordova plugin remove cordova-plugin-ionic-webview
```

#### 安装插件


**`android sdk 24 以下`，使用`crosswalk webview`浏览器内核;
`android sdk 24及以上`，使用安卓系统内置的高版本浏览器内核。**
``` shell
cordova plugin add cordova-plugin-ionic4-crosswalk-webview --variable WEBVIEW_ENGINE=AUTO
```
---

**始终使用CrosswalkWebView内核**

``` shell
cordova plugin add cordova-plugin-ionic4-crosswalk-webview --variable WEBVIEW_ENGINE=CROSSWALK
```

---

**始终使用SystemWebView内核**
``` shell
cordova plugin add cordova-plugin-ionic4-crosswalk-webview --variable WEBVIEW_ENGINE=SYSTEM
```


## 如何使用

在你的`cordova`启动页面index.html中写以下脚本

``` html
<head>
  <script>
    // 在加载cordova.js之前调用
    if (window.location.href.indexOf("file:") == 0){
      window.location.href = "http://localhost";
    }
  </script>
</head>
```

#### 切换内核


``` js
//在下次启动APP时使用CrosswalkWebView内核后
Ionic.WebView.useCrosswalkWebViewAtTheNextStartup();
```

``` js
//在下次启动APP时使用SystemkWebView内核
Ionic.WebView.useSystemWebViewAtTheNextStartup();
```

## 如何调试

由于crosswalk编译成了不同架构的apk，因此原来的命令`ionic cordova run android --emulator`不再适用，请改用以下命令调试apk

``` shell
ng run app:ionic-cordova-build --platform=android && cordova run android --emulator
```

可以使用npm封装命令，在`package.json`中

``` json
{
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "debug": "ng run app:ionic-cordova-build --platform=android && cordova run android --emulator"
  }
}
```

使用npm命令调试

``` shell
npm run debug
```

## Demo

你可以参考示例
`https://github.com/waitaction/cordova-plugin-ionic4-crosswalk-demo`