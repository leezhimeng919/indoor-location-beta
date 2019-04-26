# 思路
- 桌面应用程序开发：electron
- web应用程序开发：vue + node.js


## 高德地图
- 室内定位 SDK 
    + 适用平台：Android、IOS
    + 宣传优势
        * 高精度室内定位体验
            - 1-8m平均精度
            - 平滑跟随
            - 精准方向指示
        * 精确的垂直维度定位
            - 楼层定位
        * 多种定位方式融合
            - WiFi
            - BLE(Bluetooth Low Energy低能耗蓝牙)
            - PDR(pedestrian dead-reckoning步行者航位推算算法)
            - 地磁
    + 实现方案(需要现场定位硬件设备支持，设备部署方案)
        * WiFi
            - 定位精度：3-8m
            - 部署密度：700平1个
            - 设备单价：上百元
        * 蓝牙
            - 定位精度：1-5m
            - 部署密度：50平1个
            - 设备单价：数十元
    + 数据获取方案
        * 联系高德认证的室内数据采集厂商进行数据采集
        * 如果室内建筑未铺设WIFI或蓝牙IBeacon，则先联系高德认证的室内定位硬件部署厂家
    + 定位数据更新
        * 
- 室内地图 JS API

## 百度地图
- 百度地图api-室内图 ()[http://lbsyun.baidu.com/products/products/indoor]
- 百度地图JavaScript API-定位 ()[http://lbsyun.baidu.com/index.php?title=jspopular/guide/geolocation]
- 百度地图api-室内定位管理平台 ()[http://lbsyun.baidu.com/indoor/indoormap/introduct]
- 百度地图需要认证，身份证

## 阿里云
- 阿里云api-图像识别
    + 业务场景
        * 通过模式识别、机器学习技术分析图像，识别其中的场景和物体
    + 服务
        * 图像打标、场景识别、鉴黄
- 阿里云官方服务API校验规范
    + API整体校验规则
    ```
    Authorization =  Dataplus AccessKeyId + ":" + Signature

    Signature = Base64( HMAC-SHA1( AccessSecret, UTF-8-Encoding-Of(StringToSign) ) )

    StringToSign =
      //HTTP协议header
      HTTP-Verb + "\n" +    //GET|POST|PUT...
      Accept + "\n" +
      Content-MD5 + "\n" +  //Body的MD5值放在此处
      Content-Type + "\n" +
      Date + "\n" +
      url

    ```
    + 签名计算方法
    + 公共请求头计算签名