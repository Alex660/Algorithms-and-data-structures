## cookie 和 token 在什么情况下会被劫持？
### 常见攻击
+ CSRF
  + Cross-site request forgery（跨站请求伪造）
  + 原理是利用用户的身份，执行非用户本身意愿的操作
  + 图解
    + ![](https://upload-images.jianshu.io/upload_images/6201701-a101b1b5765c4d2c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
  + 形式
    + 图片URL
    + 超链接
    + Form提交
    + 嵌入第三方论坛、文章中
  + 防御
    + 验证码
    + 检验头部Refer字段
      + Refer用来记录该HTTP请求的来源地址
    + Token
      + 浏览器不会自带token
  + 过程
    + 服务器生成随机数Token
      + 并将该随机Token保存在用户Session（或Cookie中）
    + 用户在提交(如表单)数据时，该Token会提交到服务器
      + 服务器检测提交的Token是否与用户Session（或Cookie）中的是否一致
+ XSS攻击
  + Cross site script，即跨站脚本
  + 形式
    + 用户输入没有进行限制或过滤，从而被黑客嵌入恶意代码提交
  + 防御
    + 设置Cookie的属性为Http only，这样js就无法获取Cookie值
    + 对用户输入做过滤和限制
      + 提交数据做 Html encode处理
      + 去掉 <script>、<iframe>等特殊标签
      + 过滤js事件标签 onclick=、on...
      + 后端也要做过滤限制
### 参考解答
+ XSS下
  + Cookie、Token都有可能被劫持
+ CSRF下
  + Cookie会被劫持
  + Token不会，因为浏览器不会携带
    + 浏览器会自动带上Cookie，因为HTTP是无状态的
    + AJAX不会自动携带cookie