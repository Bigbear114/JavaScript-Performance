# JavaScript-Performance
JavaScript性能优化方面归纳

**雅虎14条优化原**
操作 | 对象
-|-
1. 尽可能的减少 HTTP 的请求数 | content
2. 使用 CDN（Content Delivery Network） | server
3. 添加 Expires 头(或者 Cache-control ) | server
4. Gzip 压缩 | server
5. 将 CSS 样式放在页面的上方 | css
6. 将脚本移动到底部（包括内联的） | javascript
7. 避免使用 CSS 中的 Expressions | css
8. 将 JavaScript 和 CSS 独立成外部文件 | javascript css
9. 减少 DNS 查询 | content
10. 压缩 JavaScript 和 CSS (包括内联的) | javascript css
11. 避免重定向 | server
12. 移除重复的脚本 | javascript
13. 配置实体标签（ETags） | css
14. 使 AJAX 缓存 | 

**1. 尽可能的减少 HTTP 的请求数**

网速相同的条件下，下载一个100KB的图片比下载两个50KB的图片要快。 

（1）分别合并CSS和JS文件；  

（2）css雪碧图（css sprites），css sprites是指只用将页面上的背景图合并成一张，然后通过css的background-position属性定义的值来取它的背景，工具：http://www.csssprites.com/，可以自动将你上传的图片合并并给出对应的background-position坐标。并将结果以png和gif的格式输出； 

（3）图片较多的页面使用懒加载，开始加载页面的时候，所有的真实图片都不去发送HTTP请求加载，而是给一张占位的背景图，当页面加载完，并且图片在可视区域内我们再去做图片加载；  

（4） 减少对于cookie的使用（最主要的是减少本地cookie存储内容的大小），因为客户端操作cookie的时候，这些信息总是在客户端和服务器端传来传去；

**2.使用 CDN（Content Delivery Network）**
通过在现有的Internet中增加一层新的网络架构，将网站的内容发布到最接近用户的 cache服务器内，通过DNS负载均衡的技术，判断用户来源就近访问cache服务器取得所需的内容，深圳的用户访问近深圳服务器上的内容，成都的就近访问成都服务器上的内容。这样可以有效减少数据在网络上传输的时间，提高速度。

**3. 添加 Expires 头(或者 Cache-control )**
通过设置Expires header 来缓存页面中的图片、脚本、css等。避免多次请求服务器。做了缓存后浏览器就不需要每次都从服务器下载文件，而是直接从缓存中读取。  
通过服务器端脚本设置Cache-Control和Expires可以完成。

**4. 启用Gzip压缩：Gzip Components**
把文件先在服务器端进行压缩，然后再传输。这样可以显著减少文件传输的大小。传输完毕后浏览器会重新对压缩过的内容进行解压缩，并执行；雅虎特别强调， 所有的文本内容都应该被gzip压缩: html (php), js, css, xml, txt… 

**5. 将 CSS 样式放在页面的上方**

**6. 将脚本移动到底部（包括内联的）**

**7. 避免使用 CSS 中的 Expressions**
```
//例如：
div { 
zoom: expression(function(el){el.style.zoom = "1"; alert(el.tagName);}(this)); 
} 
```
**8.把javascript和css都放到外部文件中 （Make JavaScript and CSS External ）**

**9.减少DNS查询 (Reduce DNS Lookups)** 

`在 Internet上域名与IP地址之间是一一对应的，域名（kuqin.com）很好记，但计算机不认识，计算机之间的“相认”还要转成ip地址。在网络 上每台计算机都对应有一个独立的ip地址。在域名和ip地址之间的转换工作称为域名解析，也称DNS查询。一次DNS的解析过程会消耗20-120毫秒的 时间,在dns查询结束之前，浏览器不会下载该域名下的任何东西。所以减少dns查询的时间可以加快页面的加载速度。yahoo的建议一个页面所包含的域 名数尽量控制在2-4个。这就需要对页面整体有一个很好的规划。`

**10.压缩 JavaScript 和 CSS (Minify JavaScript )**  

在js和css发布的时候在 服务器端进行压缩。

**11.避免重定向 (Avoid Redirects )**  

比如 当你输入http://www.kuqin.com/ 的时候服务器会自动产生一个301服务器转向 http://www.kuqin.com/ ，你看浏览器的地址栏就能看出来。这种重定向自然也是需要消耗时间的。当然这只是一个例子，发生重定向的原因还有很多，但是不变的是每增加一次重定向就会增加一次web请求，所以因该尽量减少。

**12.移除重复的脚本 (Remove Duplicate Scripts **   

代码要做到不重复，可重用

**13.配置实体标签（ETags） (Configure ETags )** 
ETags是对于某种实体的一个标识。它属于HTTP协议的一部分，所有的Web服务器都支持这个特性。
客户端（浏览器）来请求的时候，可以比较，如果ETag一致，则表示该资源并没有修改过，客户端（浏览器）可以使用自己缓存的版本。

**14. 使 AJAX 缓存**
视情况而定，多数时候做ajax请求的时候往往还要增加一个时间戳去避免他缓存。

**其他途径** 

（1）Repaint(重绘)就是在一个元素的外观被改变，但没有改变布局（宽高）的情况下发生，如改变visibility、outline、背景色等等。 
Reflow（重排）就是DOM的变化影响到了元素的几何属性，浏览器会重新计算元素的几何属性，使渲染树中受到影响的部分失效，浏览器会验证Dom树上的所有其他节点的visibility属性。如果Reflow过于频繁，CPU使用率就会很快上涨。 
减少重绘和重排： 设置样式通过class方式而不要通过style；有动画效果的元素，position属性设置为fixed或absolute，这样就不会影响其他元素的布局；减少对dom的直接操作； 

（2）减少递归的使用，避免死递归，避免由于递归导致的栈内存嵌套（建议使用尾递归）

（3）避免使用iframe（不仅不好管控样式，而且相当于在A页面中加载了其它页面，消耗较大） 

（4）页面中出现音视频标签，我们不让页面加载的时候就去加载这些资源（要不然页面加载速度会变慢）（方案：只需要设置 preload='none' 即可），等待页面加载完成，音视频播放的时候我们在去加载音视频资源

**如何评测前端性能** 

使用性能测试工具（firebug、yslow、pageSpeed等）进行，主要有以下几点：
```
1. 页面首屏加载时间
2. 页面大小、ready和loaded加载时间
3. 脚本和样式表加载个数、大小、请求时间
4. 背景图片请求数数、大小、请求时间，进行无损压缩
5. 产品图片大小、请求时间，进行无损压缩
6. 脚本、样式、图片的请求次序
7. 使用阿里测在线工具测试（http://www.alibench.com/）
8. 各ajax请求接口的时间和数据大小
9. 服务器端是否进行CDN加速
10. 服务器端是否启用Gzip压缩
11. (背景图片请求数数、大小、请求时间, 产品图片大小、请求时间)
根据评测报告，制定优化方案：
1. 提高首屏加载时间
2. 压缩css\js\html\image
3. 对脚本和样式进行CDN合并文件，减少脚本和样式文件的请求个数
4. 对背景图片使用CSS sprite，尽量控制在三张以内，并进行无损压缩
5. 结合UI交互对页面模块使用模块化按需加载
6. 提高ajax请求接口性能，可以进行合并请求的就进行合并
7. 对html\css\JS的代码进行内部优化，去掉冗余和影响性能的点

最后要对性能数据优化前后对比。。。
```