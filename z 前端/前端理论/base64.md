### [base64编码图片](https://github.com/sunbigshan/Blog/issues/43)

#### base64是什么

base64会把图片地址转化成一个长字符串，用该字符串代替图片地址，图片就会跟着html或者css一起下载到本地，而不需要另外发送一个请求

#### 优缺点

**优点**

- 能够减少一个http请求
- 加密图片。用户看不到图片的实际地址

**缺点**

- 文件体积会变大
- 需要消耗`CPU`进行编解码，会阻塞页面的渲染

#### 适用场景

base64**可以用于性能优化**，但是要合理使用。base64不一定就意味着优化性能。



[**使用方法（点我）**](https://juejin.cn/post/6844903597574995981)





















