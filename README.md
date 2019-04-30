# 目标和关键技术分析
1>目标：让用户只需要用手机相机对周围环境进行拍摄操作，就能够实现室内定位。
2>关键技术分析：
    1.计算机视觉图像分析
    1)通过手机相机获取图像，使用图像分析算法对图像进行分析(图像文字识别、场景识别)。
    2)图像文字识别：获取图片价值高的文字，在定位API的POI搜索栏中搜索来实现定位。
    3)场景识别：使用场景识别API对图片中的场景进行分析、筛选
    2.室内定位
    1)室内图显示：此处可以和WebGL相结合。
        2)定位实现：此处可以与传统WiFi指纹识别技术相结合。
    3.室内导航
    1)导航路线：此处可以和AR相结合。

# 实现思路
1. 当系统获取用户拍摄的图片后，对图片进行分析(考虑使用阿里云API，目前来看准确度不高，需要定制优化。)
2.从步骤1中分析出有用的信息，在这些信息的基础上做出定位判断。此处可以考虑调用高德地图的室内图API(demo截图如下图)。此处可以将WebGL加入室内图的实现
3.步骤2完成之后基本已经实现室内定位。此时可以输入目的地将AR与导航路线相结合。

# API
## 高德地图
- [室内地图 JS API](https://lbs.amap.com/api/javascript-api/reference/indoormap)
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

- [定位 JS API](https://lbs.amap.com/getting-started/locate)
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

- [室内定位 SDK](https://lbs.amap.com/api/android-indoorlocation-sdk/summary)
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
- [室内图](http://lbsyun.baidu.com/products/products/indoor)
    + 通用平台：Web、Android、iOS
    + 宣传优势
        * 覆盖全国4000家大型建筑
        * 分层浏览
        * POI检索
        * 路线指引
- [定位 JS API](http://lbsyun.baidu.com/index.php?title=jspopular/guide/geolocation)
    + 通用平台：Web、Android、iOS
    + 宣传优势
        * 米级室内定位
        * 定位服务全球化
        * 智能硬件定位
        * 离线定位
        * 位置语义化
        * 地理围栏
        * IP定位(HTTP协议中的定位接口，精度不高)
- [室内定位管理平台](http://lbsyun.baidu.com/indoor/indoormap/introduct)
    + 对企业级开放的API

## 阿里云
- [图像识别API](https://ai.aliyun.com/image)
    + 业务场景
        * 通过模式识别、机器学习技术分析图像，识别其中的场景和物体
    + 服务
        * 图像打标、**场景识别**、鉴黄
    + 场景识别API
- [阿里云官方服务API校验规范](https://help.aliyun.com/document_detail/30245.html)

# 阿里云场景识别测试数据
![办公室1](https://img-blog.csdnimg.cn/20190429101015252.jpg)
```
 {
    "tags":[
    {"value":"办公室","confidence":59.0},
    {"value":"室外","confidence":40.0},
    {"value":"其他","confidence":15.0},
    {"value":"演出","confidence":14.0},
    {"value":"会议室","confidence":12.0}],
    "errno":0,
    "request_id":"a6d8ed9d-b317-4421-a4f0-5b66469b8b9d"
 }
```
![室外1](https://img-blog.csdnimg.cn/20190429103609142.jpg)
```
{
    "tags":[
    {"value":"海滨","confidence":96.0},
    {"value":"小河","confidence":13.0},
    {"value":"沙滩","confidence":11.0},
    {"value":"广场","confidence":11.0},
    {"value":"室外","confidence":11.0}],
    "errno":0,
    "request_id":"0469acac-3fcf-4233-ba48-63fddce3ef05"
}
```
![办公室2](https://img-blog.csdnimg.cn/20190429103048812.jpg)
```
{
    "tags":
    [{"value":"办公室","confidence":99.0}],
    "errno":0,
    "request_id":"a693c957-51fa-49b3-9105-9eaddefed71d"
}
```
![走廊3](https://img-blog.csdnimg.cn/20190429103718380.jpg)
```
{
    "tags":
    [{"value":"物品","confidence":69.0},
    {"value":"其他","confidence":16.0},
    {"value":"演出","confidence":14.0},
    {"value":"室外","confidence":14.0},
    {"value":"卧室","confidence":14.0}],
    "errno":0,
    "request_id":"d289460f-2425-4042-a893-14aeaaa206ce"
}
```
![走廊1](https://img-blog.csdnimg.cn/20190429103733718.jpg)
```
{
    "tags":
    [{"value":"办公室","confidence":55.0},
    {"value":"会议室","confidence":20.0},
    {"value":"餐厅","confidence":16.0},
    {"value":"演出","confidence":15.0},
    {"value":"其他","confidence":14.0}],
    "errno":0,
    "request_id":"ce8e1aaf-6c56-4747-8217-87ddbee26942"
}
```
![大厅1](https://img-blog.csdnimg.cn/2019042910374890.jpg)
```
{
    "tags":
    [{"value":"物品","confidence":45.0},
    {"value":"办公室","confidence":24.0},
    {"value":"演出","confidence":23.0},
    {"value":"其他","confidence":14.0},
    {"value":"人物","confidence":13.0}],
    "errno":0,
    "request_id":"157dac48-22e6-4098-9c3e-a822865cdc1b"
}
```
![走廊2](https://img-blog.csdnimg.cn/20190429103757682.jpg)
```
{
    "tags":
    [{"value":"物品","confidence":66.0},
    {"value":"办公室","confidence":54.0},
    {"value":"室内","confidence":16.0},
    {"value":"餐厅","confidence":12.0},
    {"value":"演出","confidence":11.0}],
    "errno":0,
    "request_id":"1a555523-96de-4c2d-be84-44dfc6aca3bc"
}
```
    每个`value`都有对应的`confidence`，遍历所有列出的`value`,找到和室内相关的标签并记录



# 实现
- 桌面应用程序开发：[electron](https://electronjs.org/)
- web应用程序开发：vue + node.js

