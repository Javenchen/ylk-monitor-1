# ylk-monitor 前端监控系统（SDK）



## 使用方法

1.在html的head引入ylkMonitor.js

```html
<!DOCTYPE html>
<html>
<head>
   	...
    <script src="./path/ylkMonitor.js"></script>
</head>
```

2.初始化ylkMonitor

```js
ylkMonitor.init({
	id:1,
	url:'http://127.0.0.1:5500'
})
```

3.自动收集异常和性能数据，然后上报监控数据。




## 配置

```js
var _config = {
    id: '',  //上报id
    url: 'http://127.0.0.1:5500',   // 指定错误上报地址
    mergeReport: true, //mergeReport 是否合并上报， false 关闭， true 启动（默认）
    delay: 1000,  // 当 mergeReport 为 true 可用，延迟多少毫秒，合并缓冲区中的上报（默认）
    random: 1,  // 抽样上报，1~0 之间数值，1为100%上报（默认 1）   
    performance: { //true即开启，或者直接具体配置项
        open:true,
        random:1, //性能抽样率，不填默认为random
    },
    error: {//true即开启，或直接具体配置项
        open:true,
        random:1, //异常抽样率，不填默认为random
        ignore: [], //忽略的错误类型
    },
    waitLoadTime: 5,//五秒等待load触发，超时强行上报performance
    userInfo:'uid123' //[string|Function]
}
```



## 提供方法

**自定义队列上报**

```js
ylkMonitor.push({
    type:'diy',
    msg:'自定义队列上报'
})
```

**自定义直接上报**

```js
ylkMonitor.submit({
    type:'diy',
    msg:'自定义直接上报'
})
```



**监听回调**

```js
//上报前
ylkMonitor.on('beforeReport',function(msg){
    console.log('beforeReport',msg)
})
//成功上报后
ylkMonitor.on('aftereReport',function(msg){
    console.log('aftereReport',msg)
})
```



## 测试相关

**karma+mocha+chrome测试**

用例尚不完整

```
npm run test
```



## 上报格式

性能上报

```js

{
    "id": 1,
    "log": [{
        "type": "performance",
        "data": {
            "navigationStart": "5386175",
            "unloadEventStart": "5386183",
            "unloadEventEnd": "5386183",
            "redirectStart": "",
            "redirectEnd": "",
            "fetchStart": "5386175",
            "domainLookupStart": "5386175",
            "domainLookupEnd": "5386175",
            "connectStart": "5386175",
            "connectEnd": "5386175",
            "secureConnectionStart": "",
            "requestStart": "5386178",
            "responseStart": "5386179",
            "responseEnd": "5386181",
            "domLoading": "5386189",
            "domInteractive": "5386224",
            "domContentLoadedEventStart": "5386224",
            "domContentLoadedEventEnd": "5386224",
            "domComplete": "5386283",
            "loadEventStart": "5386283",
            "loadEventEnd": ""
        }
    }],
    "baseInfo": {
        "userAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1",
        "deviceWidth": 375,
        "deviceHeight": 667,
        "url": "http://127.0.0.1:5500/"
    },
    "userInfo": "uid123",
    "time": 1548035386284
}
```



异常上报

```js
{
    "id": 1,
    "log": [{
        "type": "error",
        "data": {
            "msg": "TypeError: ylkMonitor.isNotFunc is not a function    @ http://127.0.0.1:5500/:89:20",
            "target": "http://127.0.0.1:5500/",
            "rowNum": 89,
            "colNum": 20,
            "_orgMsg": "Uncaught TypeError: ylkMonitor.isNotFunc is not a function"
        },
        "hash": "MiS_z"
    }],
    "baseInfo": {
        "userAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1",
        "deviceWidth": 375,
        "deviceHeight": 667,
        "url": "http://127.0.0.1:5500/"
    },
    "userInfo": "uid123",
    "time": 1548035395223
}
```

