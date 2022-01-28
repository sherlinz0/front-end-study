<!--
 * @Author: sherlyzz
 * @Date: 2022-01-28
-->

# 优雅的 JS 单行代码

- 随机获取布尔值
  
  ```javascript
  const getRandomBoolean = () => Math.random() >= 0.5;
  ```

- 检查日期是否为周末
  
  ```javascript
  const isWeekend = (date) => [0, 6].indexOf(date.getDay()) !== -1;

  console.log(isWeekend(new Date(2021, 4, 14)));
  // false (Friday)
  ```

- 检查数字是偶数还是奇数
  
  ```javascript
  const isEven = (num) => num % 2 === 0;
  ```

- 获取数组中的唯一值
  
  ``` javascript
  const uniqueArr = (arr) => [...new Set(arr)];
  ```

- 检查变量是否为数组

  ```javascript
  const isArray = (arr) => Array.isArray(arr);
  ```

- 在连个数字之间生成一个随机数

  `Math.random()` 伪随机数, 范围为 `0 到小于 1`

  算法为 `(区间长度 + 1) * 随机值 + 区间左值`
  
  ```javascript
  const random = (min, max) => Math.floor(Math.random() * (max - min + 1) + min);
  ```

- 生成随机字符串
  
  `Math.random().toString` 将生成的随机数后的每个数组转换为 `0 - 9 a - z`

  `slice(2)` 从第三位开始至最后切分字符串

  ```javascript
  const randomString = () => Math.random().toString(36).slice(2);
  ```

- 滚动到页面顶部
  
  ```javascript
  const scrollToTop = () => window.scrollTo(0, 0);
  ```

- 切换布尔
  
  ```javascript
  // none return
  const toggleBool = () => (bool = !bool);
  // or
  //return bool
  const toggleBool = (b) => !b;
  ```

- 交换两个变量

  不使用第三个临时变量来交换两个变量的值.
  
  ```javascript
  [foo, bar] = [bar, foo];
  ```

- 计算两个日期之间的天数
  
  一天的毫秒数为 `86400000`
  
  ```javascript
  const daysDiff = (date1, date2) => Math.ceil(Math.abs(date1- date2) / 86400000);
  
  console.log(daysDiff(new Date('2021-05-10'), new Date('2021-11-25')));
  // 199
  ```

- 将文字复制到剪贴板
  
  可能需要添加检查查看是否存在 `navigator.clipboard.writeText`

  ```javascript
  const copyTextToClipboard = async (text) => await navigator.clipboard.writeText(text);

  ```

- 合并多个数组的不同方法
  
  - `concat`
  - `...` 拓展运算符
  
  ```javascript
  const merge = (a, b) => a.concat(b);
  //or
  const merge = (a, b) => [...a, ...b];
  ```

- 获取实际类型
  
  ```javascript
  const trueTypeOf = (obj) => Object.prototype.toString.call(obj).slice(8, -1).toLowerCase();
  ```

- 在结尾处阶段字符串
  
  ```javascript
  const truncateString = (string, length) => string.length < length ? string : `${string.slice(0, length - 3)}...`;

  console.log(
    truncateString('Hi, I should be truncated because I am too loooong!', 36),
  );
  // Hi, I should be truncated because...
  ```

- 从中间截断字符串
  
  - `string` 字符串
  - `length` 需要的长度
  - `start` 开始需要的长度
  - `end` 结束需要的长度

  ```javascript
  const truncateStringMiddle = (string, length, start, end) => `${string.slice(0, start)}...${string.slice(string.length - end)}`;

  console.log(
    truncateStringMiddle(
      'A long story goes here but then eventually ends!', // string
      25, // 需要的字符串大小
      13, // 从原始字符串第几位开始截取
      17, // 从原始字符串第几位停止截取
    ),
  );
  // A long story ... eventually ends!
  ```

- 大写字符串

  ```javascript
  const capitalize = (str) => str.charAt(0).toUpperCase() + str.slice(1);

  console.log(capitalize('hello world'));
  // Hello world
  ```

- 检查当前选项卡(页面)是否在视图 / 焦点内
  
  ```javascript
  const isTabInView = () => !document.hidden;
  ```

- 检查用户是否在 Apple 设备上
  
  `navigator.platform` 已被废弃, 虽然某些主流浏览器仍在支持,详见 [Navigator.platform](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/platform)

  ``` javascript
  const isAppleDevice = () => /Mac|iPod|iPhone|iPad/.test(navigator.platform);
  ```

- 三元运算符
  
  ```javascript
  // Longhand
  const age = 18;
  let greetings;

  if (age < 18) {
  greetings = 'You are not old enough';
  } else {
  greetings = 'You are young!';
  }

  // Shorthand
  const greetings = age < 18 ? 'You are not old enough' : 'You are young!';
  ```

- 短路评估速记

  在将变量值分配给另一个变量时, 可能要确保源变量不为 `null`, 未定义或为空.

  可以编写带有多个条件的 `long if` 语句，也可以使用短路评估.

  ```javascript
  // Longhand
  if (name !== null || name !== undefined || name !== '') {
  let fullName = name;
  }

  // Shorthand
  const fullName = name || 'buddy';
  ```

**待更新...**

**参考**

[1] **MDN** https://developer.mozilla.org/zh-CN/

[2] **学会这20+个JavaScript单行代码，可以让你的代码更加骚气** https://mp.weixin.qq.com/s/Nb0DaBN9IpJM7VgC2LG-8w