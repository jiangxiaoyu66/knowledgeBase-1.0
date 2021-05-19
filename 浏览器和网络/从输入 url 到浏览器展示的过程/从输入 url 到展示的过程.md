[浏览器输入url到显示的过程阐述？](https://github.com/ljianshu/Blog/issues/24)

[从输入 URL 到页面展示发生了什么？](https://juejin.cn/post/6931635435852529677#heading-31)

- 读取浏览器缓存
- DNS 解析
- TCP 三次握手
- 发送请求，分析 url，设置请求报文(头，主体)
- 服务器返回请求的文件 (html)
- 浏览器渲染
  - HTML parser --> DOM Tree
    - 标记化算法，进行元素状态的标记
    - dom 树构建
  - CSS parser --> Style Tree
    - 解析 css 代码，生成样式树
  - attachment --> Render Tree
    - 结合 dom树 与 style树，生成渲染树
  - layout: 布局
  - GPU painting: 像素绘制页面



# 网络请求

1.  浏览器缓存
2. DNS 解析
3. TCP 三次握手




















































