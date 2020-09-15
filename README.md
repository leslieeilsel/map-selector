<h1 align="center">百度地图描点工具</h1>

![](./assets/images/luwang.png)

### 技术栈

- jQuery
- Material based on Bootstrap v4
- ACE Editor
- 百度地图 JavaScript API
- 百度地图 鼠标绘制工具库

### Usage

```bash
git clone https://github.com/leslieeilsel/BaiduMapTool.git

# 在浏览器中打开index.html即可
```

### TODO

- [ ] BUG 修复 
- [ ] 支持转化为高德地图坐标
- [ ] 支持转化为 GPS 坐标
- [ ] 页面使用 tailwindcss 重构

### 坐标转化

- 百度官方 API

  ```shell
  http://lbsyun.baidu.com/index.php?title=webapi/guide/changeposition
  ```

- 高德官方 API

  ```shell
  https://lbs.amap.com/api/webservice/guide/api/convert/
  ```

- 自定义算法

  百度、高德提供的坐标转换是 HTTP 接口，单次响应时间在 100ms 左右。

  如果请求次数不多，这样的延迟可以忽略。

  但是在大批量转化时，就会出现较长的延迟，这时就需要使用本地算法。

  ```javascript
  // 百度坐标转高德（传入经度、纬度）
  function bd_decrypt(bd_lng, bd_lat) {
      var X_PI = Math.PI * 3000.0 / 180.0;
      var x = bd_lng - 0.0065;
      var y = bd_lat - 0.006;
      var z = Math.sqrt(x * x + y * y) - 0.00002 * Math.sin(y * X_PI);
      var theta = Math.atan2(y, x) - 0.000003 * Math.cos(x * X_PI);
      var gg_lng = z * Math.cos(theta);
      var gg_lat = z * Math.sin(theta);
      return {lng: gg_lng, lat: gg_lat}
  }
  
  // 高德坐标转百度（传入经度、纬度）
  function bd_encrypt(gg_lng, gg_lat) {
      var X_PI = Math.PI * 3000.0 / 180.0;
      var x = gg_lng, y = gg_lat;
      var z = Math.sqrt(x * x + y * y) + 0.00002 * Math.sin(y * X_PI);
      var theta = Math.atan2(y, x) + 0.000003 * Math.cos(x * X_PI);
      var bd_lng = z * Math.cos(theta) + 0.0065;
      var bd_lat = z * Math.sin(theta) + 0.006;
      return {
          bd_lat: bd_lat,
          bd_lng: bd_lng
      };
  }
  ```
  以下是 PHP 版本的算法
  
  ```shell
  https://beltxman.com/archives/1628.html
  ```
