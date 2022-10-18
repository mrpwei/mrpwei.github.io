# HTML5

## href 与 src 的区别

相同点：都是用来调用资源的
不同点：

1. 引用不同：href 是超文本引用，用来建立当前元素和文档之间的链接，常用有`<link>`和`<a>`，而 src 是将指向的资源下载并应用到文档中，用来替换当前内容，常用有`<script>`和`<img>`
2. 渲染机制不同：href 会跟 html 解析同时渲染，不会停止对文档的处理，而 src 是要暂停其他资源的下载和处理，直到解析并渲染为此，所以 js 一般放在底部。