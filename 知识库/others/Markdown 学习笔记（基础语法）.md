# Markdown 学习笔记（基础语法）

* Headers

  # this is an <h1> tag/# [text]

  ## this is an <h2> tag/## [text]

* Emphasis

  *this text is italic*/\*[text]\*/\_[text]_

  **this text is bold**/\*\*[text]\*\*\_\_[text]_\_

  ***this text is italic and bold***/\*\*\*[text]\*\*\*/\_\_\_[text]_\_\_

  

* List

  * Unordered
    * item1/* [item1]
    * item2
    * item3

  * Ordered
    1. item1/1. [item1]
    2. item2
    3. item3

* Images

  ![百度](https://www.baidu.com/img/PC_7ac6a6d319ba4ae29b38e5e4280e9122.png)
  Format: ![Alt Text](url)

  

* Links

  http://github.com - automatic!

  [GitHub](http://github.com)/\[GitHub](http://github.com)

* Blockquotes

  As Kanye West said:

  > We're living the future so/ \> [text]
  > the present is our past.

* Inline code

  I think you should use an
  `<addr>`/ \`\<text\>\` element here instead.

* Syntax highlighting

  ```javascript
  console.log("markdown")
  ```

  ````
  ```javascript
  function fancyAlert(arg) {
    if(arg) {
      $.facebox({div:'#foo'})
    }
  }
  ```
  ````

* Task List
  - [ ] item1/ \-\[ \] [text]
  - [x] item2/ \-\[x\] [text]