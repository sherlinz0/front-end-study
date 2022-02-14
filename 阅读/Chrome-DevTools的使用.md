<!--
 * @Author: shelryzz
 * @Date: 2022-01-28
 * @LastEditTime: 2022-01-28 22:42:35
 * @LastEditors: Please set LastEditors
 * @Description: 介绍 Chrome 浏览器开发者工具的使用
-->

# Chrome DevTools 的使用

## Console

- 在任意面板唤起 `console`
  
  ```Ctrl + ` ```

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1a771ff6b9e24d75a001f0080f323a0c~tplv-k3u1fbpfcp-watermark.awebp)

- `$0` `$1` `$_`
    
    - `$0`: 引用最后一次选中的 `DOM` 节点
    - `$1`: 引用倒数第二个选中的 `DOM` 节点
    - `$_`: 引用 `console` 中上一个表达式返回的值

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9be0931f5e034f42999aa71b42ac8b0b~tplv-k3u1fbpfcp-watermark.awebp)

- store as global variable

  - 当想将某些值存储到变量 (接口请求, 返回参数, dom节点, debug时的某些变量) 在 `console` 面板使用时.
  
  - 当在 `console` 打印出一些库的对象时, 使用 `store as global variable` 后能够直接调用这些库的方法, 比如 `moment`, `antd` 的 `form` 等.
  
  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/19ec26a37e694f77bd69c10a062788ef~tplv-k3u1fbpfcp-watermark.awebp)

- copy()
  
  帮助快速 copy 一些 `console` 中的变量

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/368b0a73ef7e4a26bb783256624e8718~tplv-k3u1fbpfcp-watermark.awebp)

## Elements

- computed style
  
  当项目复杂时, 一个节点上的样式可能会受到多处代码的影响, `computed style` 可以查看使该节点生效的样式.

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5bc2bb5858de4c0e8f28962612be0ce6~tplv-k3u1fbpfcp-watermark.awebp)

- Break on

  当 `DOM`状态变化时大断点

  使用场景:\
  在中后台项目中经常会基于 `antd` 的组件做一些改造, 以 `Dropdown Menu` 举例, 我们想自定义改造组件下拉的内容, 来满足我们的需求. 这时候如果直接使用检查元素来修改, 会发现鼠标一旦移开后下拉就会收起, 造成了调试的麻烦. 这个时候就很适合使用 `Break on` 这个功能.
  
  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/807cd222e78c45929ca36bbbc775f815~tplv-k3u1fbpfcp-watermark.awebp)

- 提取节点样式
  
  `右击节点` -> `copy` -> `copy styles`

## Network

- 禁用网络缓存
  
  当新内容发布后, 页面始终展示的还是旧的内容时使用(开发调试时最好一直将其勾选)

  `Network` -> `Disable cache`

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/71bcfe8da81846fbaf799606db0729d0~tplv-k3u1fbpfcp-watermark.awebp)

- 网络节流 (模拟弱网环境)
  
  当开发移动端应用时, 查看应用在弱网环境下是否还能够有较好的加载速度

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06ec9222a42648039b90f236d26f2550~tplv-k3u1fbpfcp-watermark.awebp)

- Block url
  
  组织某个 `url` 的请求 (当不得不在线上 debug 时, 把某些写操作的请求给组织, 如表单提交等)

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a8d208d043f144a3ab7eeb1e60f62982~tplv-k3u1fbpfcp-watermark.awebp)

- advance filter
  
  对网络请求进行更精细的筛选

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1effee2f37c44d8794aeacb2e6df46dc~tplv-k3u1fbpfcp-watermark.awebp)

  - is:form-cache
  
    查看哪些请求时走缓存的

  - has-response-header:
  
    筛选出包含某些请求头的接口

  - larger-than:
    
    查看有哪些请求体积过大, 便于分析性能, 优化拆包等

  - method

    只查看某种方法的网络请求 (POST / GET)

  - 其他的
    
    输入英文首字母, 自动提示

- Header options
  
  在 `network` 面板, 右击, 可以选择在 `network` 面板中展示的内容 (`cookie`, `cache-control`)

  可以将接口某个自定义的 `header` 展示出来, 如常见的 `x-tt-logid`

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7a6b6ac69e0342f5bbc5d811b996fcb5~tplv-k3u1fbpfcp-watermark.awebp)

## Source Panel

- Conditional break
  
  当我们在某处打了个断点, 却只想在某些情况下触发时使用

  使用场景:\
  当我们打断点调试 `react` 代码时, 经常会遇到 `state` 变化导致页面多次触发 `render`, 从而一直触发断点, 而我们只想在特定条件才想断点的情况. 下面这个例子示范了如何只在 `num` 有值的情况下才断点.

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4a9418ba705d41cf896b99778ecc7880~tplv-k3u1fbpfcp-watermark.awebp)

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b0dde7f9885040528a4fdd7a0228c136~tplv-k3u1fbpfcp-watermark.awebp)

- Blackbox (add script to ignore list)
  
  跳过工具库的的代码 (在调用栈上不显示 `react`, `react-dom` 等工具的函数)

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4389931ff9544d6d97552d71e6068d95~tplv-k3u1fbpfcp-watermark.awebp)

- Pause on exception
  
  当出现抛出错误时候断点

  使用场景:\
  当代码因为某些情况 (接口数据异常, 访问了 `undefined` 等) 导致页面白屏, 可以使用此方法在出现异常的上下文环境查看异常的原因

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0fe6e75aed62497b8431ff7dc395bede~tplv-k3u1fbpfcp-watermark.awebp)

- Never Pause Here
  
  当在某处打了断点, 但是暂时不想关心这里的具体情况

  结合上面说到的 Pause on exception, 如果开启这个的话会在任何有抛错的地方打断点, 但有时候像一些老项目里有很多报错, 但是不影响正常逻辑时, 我们可以用这个方法忽略这个断点

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9757731fff45464584a377358dd5bd84~tplv-k3u1fbpfcp-watermark.awebp)

- Code Folding
  
  `Ctrl + Shift + p` 可以将函数体收起

## Other

- `Ctrl + Shift + p`
  
  在任意面板按下这三个键, 会弹出功能列表, 可以快捷的跳转到某些面板, 使用某些功能等..

- Show paint rectangles

  高亮显示页面哪个区块在渲染

- screenshot

  对某个 `DOM` 节点 / 全屏 进行截图

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ef8ab02e56984b87838df8ab9dbd69b1~tplv-k3u1fbpfcp-watermark.awebp)

**参考**

[1] **实用 Chrome DevTool Tips** https://juejin.cn/post/6987752907579850765