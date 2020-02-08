## DNS解析
### 定义
+ 是一个递归查询的过程
### 参考文献
+ [DNS解析的过程是什么，求详细的？](https://www.zhihu.com/question/23042131?sort=created)
### 图解
+ ![](https://user-gold-cdn.xitu.io/2018/12/22/167d5d6b03fe52c5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
### 简析
1. 浏览器先检查本地DNS缓存
2. 没有，则去本机 hosts 解析
3. 也没有，即 hosts 文件没有找到，则会向(LDNS)本地区域名服务器发起域名解析请求
   1. 如运营商
   2. 请求一般用 UDP 高效，失败或其它情况时用 TCP
4. 本地运营商缓存也没找到，则向根域的DNS的IP地址（这个DNS服务器都内置13台根域的DNS的IP地址）发起请求（请问www.google.com这个域名的IP地址是多少啊？）
5. 根域名服务器返回给本地域名服务器一个所查询域的主域名服务器(gTLD Server)地址，gTLD是国际顶级域名服务器，如.com、.cn、.org等，全球只有13台左右。
   1. 根域发现这是一个顶级域com域的一个域名，
   2. 于是就告诉运营商的DNS我不知道这个域名的IP地址，但是我知道com域的IP地址，你去找它去。
6. 本地域名服务器(Local DNS Server)再向上一步返回的gTLD服务器发送请求。
   1. 于是运营商的DNS就得到了com域的IP地址，
   2. 又向com域的IP地址发起了请求（请问www.google.com这个域名的IP地址是多少?），
   3. com域这台服务器告诉运营商的DNS我不知道www.google.com这个域名的IP地址，但是我知道google.com这个域的DNS地址，你去找它去。
7. 接受请求的gTLD服务器查找并返回此域名对应的Name Server域名服务器的地址，这个Name Server通常就是你注册的域名服务器
   1. 这个时候google.com域的DNS服务器一查，果真在我这里，于是就把找到的结果发送给运营商的DNS服务器，
   2. 这个时候运营商的DNS服务器就拿到了www.google.com这个域名对应的IP地址。
8. Name Server域名服务器会查询存储的域名和IP的映射关系表，正常情况下都根据域名得到目标IP记录，连同一个TTL值返回给DNS Server域名服务器。
9. 返回该域名对应的IP和TTL值，Local DNS Server会缓存这个域名和IP的对应关系，缓存的时间由TTL值控制。
10. 把解析的结果返回给用户，用户根据TTL值缓存在本地系统缓存中，域名解析过程结束。