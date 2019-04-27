# 思路
- 桌面应用程序开发：electron
- web应用程序开发：vue + node.js

## 高德地图
- (室内地图 JS API)[https://lbs.amap.com/api/javascript-api/reference/indoormap]
    + 通用平台：Web、Android、iOS
    + 宣传优势
        * 室内地图渲染
        * 室内POI(兴趣点)搜索(TH：可以利用这一点。计算机视觉识别图像中的文字，将这些文字传到POI搜索框进行定位)
        * 室内路线规划
    + 实现
        * 类: AMap.IndoorMap 室内地图插件
        * Building对象
            - floor：所在楼层
            - name：楼层名称
            - lnglat：楼层经纬度
            - id：所属楼宇信息
        * Shop对象
            - id：店铺id
            - name：店铺名称
            - lnglat：店铺经纬度
            - building_id：店铺所属楼宇信息

- (定位 JS API)[https://lbs.amap.com/getting-started/locate]
    + 通用平台：Web、Android、iOS
    + 宣传优势
        * 基于多途径的混合定位方式
            - 定位方式
                + GPS
                + 基站
                + WiFi
                + 蓝牙
                + 传感器
            - 混合定位方式的优点
                + 毫秒级响应
                + 毫安级耗电
                + 十米内精度
                + 无GPS依赖
        * 室内室外定位一体化，室内定位米级突破
        * 日均千亿级定位请求，10部手机中9部使用高德位置服务
        * 地理围栏实现精准推送
            - 高德POI
            - 行政区划分
            - 自定义圆形
            - 多边形
    + 实现
        * 类
            - AMap.Geolocation 定位插件
            - AMap.CitySearch 城市查询，IP定位获取当前城市信息插件
        * GeolocationResult对象
        * CitySearchResult对象
            - city：城市名称
            - bounds：地图展示该城市时的矩形区域

- (室内定位 SDK)[https://lbs.amap.com/api/android-indoorlocation-sdk/summary] 
    + 通用平台：Android、IOS
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
        * 半年一次


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