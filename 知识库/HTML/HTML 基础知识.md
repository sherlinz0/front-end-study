# HTML 基础知识

> HTML 不是一门编程语言，而是一种用于定义内容结构的*标记语言*。HTML 由一系列的**元素（[elements](https://developer.mozilla.org/zh-CN/docs/Glossary/Element)）**组成，这些元素可以用来包围不同部分的内容，使其以某种方式呈现或者工作。 一对标签（ [tags](https://developer.mozilla.org/zh-CN/docs/Glossary/Tag)）可以为一段文字或者一张图片添加超链接，将文字设置为斜体，改变字号，等等。
>
> -MDN

### 元素：

由开始标签和结束标签包围起来组成一个元素，元素的包含的内容可以为空，元素也可以嵌套其他元素

### 属性：

元素可以有属性，attribute="value",熟悉值最好由双引号包围，便于阅读，如果下列情况，可不用引号，但最好用引号。

> 不包含 ASCII 空格（以及 `"` `'` ``` `=` `<` `>` ）的简单属性值可以不使用引号，但是建议将所有属性值用引号括起来，这样的代码一致性更佳，更易于阅读。
>
> -MDN

### 来看一些元素如何组合一个页面：

>``` html
><!DOCTYPE html>
><html>
>  <head>
>    <meta charset="utf-8">
>    <title>测试页面</title>
>  </head>
>  <body>
>    <img src="images/firefox-icon.png" alt="测试图片">
>  </body>
></html>
>```
>
>- `<!DOCTYPE html>` — 文档类型。混沌初分，HTML 尚在襁褓（大约是 1991/92 年）之时，`DOCTYPE` 用来链接一些 HTML 编写守则，比如自动查错之类。`DOCTYPE` 在当今作用有限，仅用于保证文档正常读取。现在知道这些就足够了。
>- `<html></html>` — [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/html) 元素。该元素包含整个页面的内容，也称作根元素。
>- `<head></head>` — [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/head) 元素。该元素的内容对用户不可见，其中包含例如面向搜索引擎的搜索关键字（[keywords](https://developer.mozilla.org/zh-CN/docs/Glossary/Keyword)）、页面描述、CSS 样式表和字符编码声明等。
>- `<meta charset="utf-8">` — 该元素指定文档使用 UTF-8 字符编码 ，UTF-8 包括绝大多数人类已知语言的字符。基本上 UTF-8 可以处理任何文本内容，还可以避免以后出现某些问题，没有理由再选用其他编码。
>- `<title></title>` — [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/title) 元素。该元素设置页面的标题，显示在浏览器标签页上，也作为收藏网页的描述文字。
>- `<body></body>` — [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/body) 元素。该元素包含期望让用户在访问页面时看到的内容，包括文本、图像、视频、游戏、可播放的音轨或其他内容。

### 图像

``` html
<img src="images/firefox-icon.png" alt="测试图片">
```

src为图片路径属性，可以为本地图片，也可以为网络图片，alt为替换文字属性，是图像的描述内容，用于图片不能可见时显示。

不可见的原因有：

人为原因：用户有视觉障碍，有alt属性时可以通过阅读器知道该图片的内容

客观原因：图片路径错误等

### 标记文本

1. 标题

   标题有6个级别h1 - h6 用法: \<hn\> content \<hn\>

   注：不要使用标题元素来加大字体，因为标题对 无障碍阅读 和 搜索引擎优化 问题具有重要意义

2. 段落

   \<p\>元素用来指定段落。通常用于显示常规文本内容

3. 列表

   1. 有序列表 \<ol\> (Ordered List)
   2. 无序列表 \<ul\> (Undered List)

   注：列表的每个项目都用一个列表项目(List Item)元素\<li\>包围

4. 链接

   > 链接非常重要 — 它们赋予 Web 网络属性。要植入一个链接，我们需要使用一个简单的元素 — [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a) — a 是 "anchor" （锚）的缩写。
   >
   > -MDN

   用法如下：

   ``` html
   <a href="https://www.mozilla.org/zh-CN/about/manifesto/">Mozilla 宣言</a>
   href -hypertext reference
   ```

   