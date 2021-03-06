# LeetCode 每日

LeetCode 的每日刷题记录, 记录题解以及思路.

## 2022-02-10

### [两数之和](https://leetcode-cn.com/problems/two-sum/)

- 使用 Array.prototype.indexOf()

    遍历数组中每一个元素, 使用 Array.prototype.indexOf() 来确定 `target - nums[i]` 值存在与否.

    ```javascript
    /**
     * @param {number[]} nums
     * @param {number} target
     * @return {number[]}
     */
    var twoSum = function(nums, target) {
        let result = [];
        
        for(let i = 0; i < nums.length; i++) {
            let anotherIndex = nums.indexOf(target - nums[i]);
            if(anotherIndex !== -1 && anotherIndex !== i) return [i, anotherIndex];
        }
        return [];
    };
    ```
  
- 哈希映射

    遍历数组中每一个元素, 建一个空 map, 每次访问元素时, 检查 map 中是否存在 `target - nums[i]` 这个键, 若不存在则将 `nums[i]: i` 键值对存入 map 中.

    ```javascript
    /**
     * @param {number[]} nums
     * @param {number} target
     * @return {number[]}
     */
    var twoSum = function(nums, target) {
        let map = {};
        for(let i = 0; i < nums.length; i++) {
            if(map[target - nums[i]] !== undefined) return [map[target - nums[i]], i];
            else map[nums[i]] = i; 
        }
        return [];
    };
    ```
  
### [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

- 倒序双指针

    ```javascript
    /**
     * @param {number[]} nums1
     * @param {number} m
     * @param {number[]} nums2
     * @param {number} n
     * @return {void} Do not return anything, modify nums1 in-place instead.
     */
    var merge = function(nums1, m, nums2, n) {
        // 倒序比较, 标记两个数组元素的下标
        let [index1, index2] = [m - 1, n - 1];
        // 标记正在放置元素的下标
        let index3 = nums1.length - 1;
    
        // 倒序比较, 放置元素
        while(index1 !== -1 && index2 !== -1) {
            if(nums1[index1] < nums2[index2]) nums1[index3--] = nums2[index2--];
            else nums1[index3--] = nums1[index1--];
        }
        
        // 肯定会出现一个数组的元素没放完
        while(index1 !== -1) nums1[index3--] = nums1[index1--];
        while(index2 !== -1) nums1[index3--] = nums2[index2--];
    };
    ```

### [三数之和](https://leetcode-cn.com/problems/3sum/)

- 动态双指针 + 固定单指针

    先将数组排序, 用一个单指针指向外层循环访问的数组元素, 另外两个指针指向固定指针后面的两端. 过程中排除重复元素.

    ```javascript
    /**
     * @param {number[]} nums
     * @return {number[][]}
     */
    var threeSum = function(nums) {
        const length = nums.length;
        const result = [];
    
        // 源数组不满足条件, 直接返回
        if(length < 3) return result;
    
        // 先将数组排序
        nums.sort((a, b) => a - b);
    
        for(let i = 0; i < length; i++) {
            // 若 nums[i] > 0, nums[L] 和 nums[R] 必定大于 0
            if(nums[i] > 0) break;
            // 当 nums[i] 和 nums[i - 1] 相等时, 跳过, 排除重复
            if(i > 0 && nums[i] === nums[i - 1]) continue;
    
            let L = i + 1;
            let R = length - 1;
    
            while(L < R) {
                const sum = nums[i] + nums[L] + nums[R];
    
                if(sum === 0) {
                    result.push([nums[i], nums[L], nums[R]]);
                        
                    // 排除重复
                    while(L < R && nums[L] === nums[++L]);
                    while(L < R && nums[R] === nums[--R]);
                }
                // 移动 L, R
                else if(sum < 0) L++;
                else if(sum > 0) R--;
                }
            }
        
        return result;
    };
    ```

## 2022-02-11

### [有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

- 二分查找

  根据题目提示, 时间复杂度为 O(logn), 且数组为有序的, 所以使用二分查找.\
  目标值最明确的一个特征是该值两侧的元素数量都为偶数, 所以根据这个特征, 将 nums[mid] 与 nums[mid + 1], nums[mid - 1] 比较, 调整左右边界, 直至找出目标.

  ```javascript
  /**
   * @param {number[]} nums
   * @return {number}
   */
  var singleNonDuplicate = function(nums) {
      let len = nums.length;
      let mid = Math.floor(len / 2);
  
      // 当数组只有一个元素时, 该元素就是目标元素
      if(len === 1) {
          return nums[0];
      }
  
      // 判断 mid 为偶数还是奇数
      if(mid % 2 === 0) {
          // 判断 mid 与左右两侧的关系
          // 调整边界, 递归调用
          if(nums[mid] === nums[mid - 1]) return singleNonDuplicate(nums.slice(0, mid + 1));
          else if(nums[mid] === nums[mid + 1]) return singleNonDuplicate(nums.slice(mid));
          else return nums[mid];
      } else {
          if(nums[mid] === nums[mid + 1]) return singleNonDuplicate(nums.slice(0, mid));
          else if(nums[mid] === nums[mid - 1]) return singleNonDuplicate(nums.slice(mid + 1));
      }
  };
  ```

### [数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

- 哈希映射

  使用一个哈希表来记录每个元素出现的次数, 最后遍历这个哈希表, 将出现次数为 1 的元素添加到数组中.

  ```javascript
  /**
   * @param {number[]} nums
   * @return {number[]}
   */
  var singleNumbers = function(nums) {
      // 哈希映射, key 为 元素值, value 为 元素值状态, 0 为 出现两次， 1 为出现 一次
      let map = {};
      let result = [];
  
      for(let e of nums) {
          // 更新状态
          if(!map[e]) map[e] = 1;
          else map[e] = 0;
      }
  
      // 便利 map 属性, 访问 value, 将 value 为 1 的添加到数组中
      for(let e in map) {
          if(map[e]) result.push(e);
      }
  
      return result;
  };
  ```

### [只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)

- 哈希映射

  使用一个哈希表来记录每个元素出现的次数, 最后遍历这个哈希表, 将出现次数为 1 的元素添加到数组中.

  ```javascript
  /**
   * @param {number[]} nums
   * @return {number}
   */
  var singleNumber = function(nums) {
      let map = new Map();
      let result = [];
  
      for(let e of nums) {
          if(map.has(e)) {
              let counter = map.get(e);
              counter++;
              map.set(e, counter);
          } else {
              map.set(e, 1);
          }
      }
      
      for(let [key, value] of map) {
          if (value === 1) result.push(key);
      }
  
      return result;
  };
  ```

### 小结

记录数组中元素出现的次数都可以用哈希表来记录, 最简单的方法．

## 2022-02-12

### [存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

- Set 类型去重

  ```javascript
  /**
   * @param {number[]} nums
   * @return {boolean}
   */
  var containsDuplicate = function(nums) {
      // 先用 Set 去重, 再将 Set 转化为数组, 最后比较 length
      return Array.from(new Set(nums).keys()).length !== nums.length;
  };
  ```

- 排序, 左右判重

  先将数组排序, 这时每次访问数组元素时只需判断该元素与左侧或右侧相邻元素是否相等即可

  ```javascript
  /**
   * @param {number[]} nums
   * @return {boolean}
   */
  var containsDuplicate = function(nums) {
      nums.sort();
      for(let i = 0; i < nums.length - 1; i++) {
          if(nums[i] === nums[i + 1]) return true;
      }
      return false;
  };
  ```

### [最大子数组和](https://leetcode-cn.com/problems/maximum-subarray/)

- 维护最大值

  ```javascript
  /**
   * @param {number[]} nums
   * @return {number}
   */
  var maxSubArray = function(nums) {
      // 用一个 pre 来暂时记录子数组的和
      let pre = 0;
      // 避免出现只有一个元素且为负数而 sum = 0
      let sum = -Infinity;
  
      for(let i = 0; i < nums.length; i++) {
          pre = pre + nums[i];
  
          if(pre > sum) sum = pre;
          // 当 pre 小于 0 时, 直接丢弃之前的状态, 更新 pre 为 0
          if(pre < 0) pre = 0;
      }
      return sum;
  };
  ```

### [买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

- 维护 最大利益 和 最小价格

  要找到最大差值, 我们只需更新最小价格, 每次访问数组元素时, 将元素值减去最小价格, 若大于最大利益, 则更新最大利益.   
  
  ```javascript
  /**
   * @param {number[]} prices
   * @return {number}
   */
  var maxProfit = function(prices) {
      let lowestPrice = prices[0];
      let maxProfit = 0;
  
      for(let price of prices) {
          if(price < lowestPrice) lowestPrice = price;
          if(price - lowestPrice > maxProfit) maxProfit = price - lowestPrice;
      }
  
      return maxProfit;
  };
  ```

### 小结

买卖股票的最佳时机 是 基于最大子数组 和的改编, 两个题的思想都是要维护状态, 用先前状态与目前状态比较, 更新状态.

## 2022-02-13

### [两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

- 两层循环

  两层循环比较元素, Array.prototype.splice() 去除 nums2 已相等的元素  

  ```javascript
  /**
   * @param {number[]} nums1
   * @param {number[]} nums2
   * @return {number[]}
   */
  var intersect = function(nums1, nums2) {
      let result = [];
  
      for(let e of nums1) {
          for(let i = 0; i< nums2.length; i++) {
              if(e === nums2[i]) {
                  result.push(e);
                  nums2.splice(i, 1);
                  break;
              }
          }
      }
  
      return result;
  };
  ```
  
- 排序 + 双指针

  先排序, 之后利用双指针来比较元素.

  ```javascript
  /**
   * @param {number[]} nums1
   * @param {number[]} nums2
   * @return {number[]}
   */
  var intersect = function(nums1, nums2) {
      let result = [];
      // 定义两个指针
      let i1 = 0, i2 = 0;
      // 定义排序方法
      let sortMethod = (a, b) => a - b;
  
      nums1.sort(sortMethod); nums2.sort(sortMethod);
  
      while(i1 < nums1.length && i2 < nums2.length) {
          // 当 nums1[i1] < nums2[i2] 时, 需要移动指针 i1 至 nums1[i1] >= nums2[i2], 直接将前面的排除掉, 不可能相等
          while(nums1[i1] < nums2[i2]) i1++;
  
          if(nums1[i1] === nums2[i2]) {
              result.push(nums1[i1]);
              i1++; i2++;
          }else {
              // nums1[i1] > nums2[i2] 时, 指针 i2 向后移动一位
              i2++;
          }
      }
  
      return result;
  };
  ```

### [重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

- 数组扁平化

  将 mat 扁平化, 用一个数组 temp 保存, 最后依次将 temp 中元素按 每次 c 个转移到 result 数组中.

  ```javascript
  /**
   * @param {number[][]} mat
   * @param {number} r
   * @param {number} c
   * @return {number[][]}
   */
  var matrixReshape = function(mat, r, c) {
      if(mat.length * mat[0].length !== r * c) return mat;
  
      let result = [], temp = [];
  
      mat.forEach((row) => {
          temp = temp.concat(row);
      })
  
      while(temp.length !== 0) result.push(temp.splice(0, c));
  
      return result;
  };
  ```

### [杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)

- 模拟

  模拟每一步, 直接求解

  ```javascript
  /**
   * @param {number} numRows
   * @return {number[][]}
   */
  var generate = function(numRows) {
	  let result = [];
  
	  for(let i1 = 0; i1 < numRows; i1++) {
		  if(i1 < 2) result.push(Array(i1 + 1).fill(1));
		  else {
			  let row = Array(i1 + 1).fill(1);
  
			  for(let i2 = 1; i2 < i1; i2++) {
                row[i2] = result[i1 - 1][i2 - 1] + result[i1 - 1][i2];
			  }
  
			  result.push(row);
		  }
	  }
	  return result;
  };
  ```

## 2022-02-14

### [最大数](https://leetcode-cn.com/problems/largest-number/)

- 局部最大

  ```javascript
  /**
   * @param {number[]} nums
   * @return {string}
   */
  var largestNumber = function(nums) {
      // 排序方法: 使局部最大, [b, a], 使得 String(b) + String(a) > String(a) + String(b)
      const sortMethod = (a, b) => {
          const string1 = String(a) + String(b);
          const string2 = String(b) + String(a);
  
          if(string1 < string2 || string1 === string2) return 0;
          else return -1;
      };
      
      nums.sort(sortMethod);
  
      // String -> BigInt( 不使用 parseInt(), 防止出现大数, 同时当 [0, 0]时, 能除去多余的 0 ) -> String( 题目要求 )
      return String(BigInt(nums.join('')));
  };
  ```

### [反转字符串](https://leetcode-cn.com/problems/reverse-string/)

- 组合 API

  ```javascript
  /**
   * @param {character[]} s
   * @return {void} Do not return anything, modify s in-place instead.
   */
  var reverseString = function(s) {
      return s.reverse().join('');
  };
  ```

- 双指针

  用两个指针分别指向字符串头和尾, 再利用 解构赋值 进行交换.

  ```javascript
  /**
   * @param {character[]} s
   * @return {void} Do not return anything, modify s in-place instead.
   */
  var reverseString = function(s) {
      let beginIndex = 0;
      let endIndex = s.length - 1;
  
      while(beginIndex < endIndex) {
          [s[beginIndex], s[endIndex]] = [s[endIndex], s[beginIndex]];
          beginIndex++;
          endIndex--;
      }
  };
  ```

### [验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

- 正则匹配 + 数组反转

  先使用正则匹配处理无关字符, 之后使用 reverse() 反转字符数组, 最后转化为字符串进行比较.

  ```javascript
  /**
   * @param {string} s
   * @return {boolean}
   */
  var isPalindrome = function(s) {
      let newStr = s.toLowerCase().match(/[a-z0-9]+/g);
      // 空字符串直接返回 true
      if(!newStr) return true;
      // String === (String -> Array -> Array -> String)
      return newStr.join('') === newStr.join('').split('').reverse().join('');
  };
  ```

- 双指针 + 字符判断函数

  若遇到无效字符, 则移动双指针跳过

  ```javascript
  /**
   * @param {string} s
   * @return {boolean}
   */
  var isPalindrome = function(s) {
	  // 消除大小写影响
      const newStr = s.toLowerCase();
      let beginIndex = 0;
      let endIndex = s.length - 1;
  
      while(beginIndex < endIndex) {
          if(!isValid(newStr.charAt(beginIndex))) {
              beginIndex++;
              continue;
          }
          if(!isValid(newStr.charAt(endIndex))) {
              endIndex--;
              continue;
          }
          if(newStr.charAt(beginIndex) !== newStr.charAt(endIndex)) return false;
        
          beginIndex++;
          endIndex--;
      }
      return true;
  };
  
  // 判断字符是否在 字母 / 数组范围内 ( 是否有效 )
  const isValid = (str) => str >= 'a' && str <= 'z' || str >= '0' && str <= '9';
  ```

### 小结

- 最大数采用了 局部到整体 的思想, 先局部实现最大, 最后整体实现最大.
- 反转字符串类的题, 可以使用 `String.prototype.split('')` + `Array.prototype.reverse()` + `Array.prototype.join('')` 三件套实现 `String -> Array -> Array ( reversed ) -> String ( reversed )` 的转化.
- 验证回文串最简单的方法是 正则匹配 + 数组反转 的组合.

## 2022-02-15

### [反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

- 模拟 + 组合 API

  直接模拟操作, 使用组合 API 对字符串进行反转

  ```JavaScript
  /**
   * @param {string} s
   * @param {number} k
   * @return {string}
   */
  var reverseStr = function(s, k) {
      const length = s.length;
      let newStr = '';
      let i = 2 * k;
  
      for(; i <= length; i += 2 * k) {
          newStr += s.slice(i - 2 * k, i - k).split('').reverse().join('') + s.slice(i - k, i);
      }
      
      // 回退上一个状态
      i -= 2 * k;
      
      if(length - i < k) newStr += s.slice(i).split('').reverse().join('');
      if(length - i < k * 2 && length - i >= k) newStr += s.slice(i, i + k).split('').reverse().join('') + s.slice(i + k);
  
      return newStr;
  };
  ```

### [验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)

- 两次验证

  ```JavaScript
  /**
   * @param {string} s
   * @return {boolean}
   */
  var validPalindrome = function(s) {
            // 定义双指针
            let leftPointer = 0;
            let rightPointer = s.length - 1;
  
            while(leftPointer < rightPointer) {
              // 当左右指针指向的值不相等时, 去掉一个元素再进行判断
              if(s.charAt(leftPointer) !== s.charAt(rightPointer)) 
                  return isPalindrome(s.slice(leftPointer + 1, rightPointer + 1)) || isPalindrome(s.slice(leftPointer, rightPointer));
  
              leftPointer++;
              rightPointer--;
            }
  
            return true;
          };
  
  const isPalindrome = (s) => {
    // 定义双指针
    let leftPointer = 0;
    let rightPointer = s.length - 1;
  
    while(leftPointer < rightPointer) {
      if(s.charAt(leftPointer++) !== s.charAt(rightPointer--)) return false;
    }
    return true;
  }
  ```

### [字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

- 哈希映射

  利用哈希表存储每个字母出现次数

  ```JavaScript
  /**
   * @param {string} s
   * @return {number}
   */
  var firstUniqChar = function(s) {
      const appearTimes = new Map();
  
      for(let c of s) {
          if(appearTimes.has(c)) appearTimes.set(c, appearTimes.get(c) + 1);
          else appearTimes.set(c, 1);
      }
  
      for(let [char, time] of appearTimes) {
          if(time === 1) return s.indexOf(char);
      }
  
      return -1;
  };
  ```
  
### 小结

题目都比较简单, 有时候可以模拟操作, 直接解题.

## 2022-02-16 

### [赎金信](https://leetcode-cn.com/problems/ransom-note/)

- 哈希映射

  建立一个哈希表 `lettersPool`, 用于存放 `magazine` 中出现的 字母 以及 对应数量

  ```javascript
  /**
   * @param {string} ransomNote
   * @param {string} magazine
   * @return {boolean}
   */
  var canConstruct = function(ransomNote, magazine) {
      const lettersPool = new Map();
      
      for(let letter of magazine) {
          if(lettersPool.has(letter)) {
              lettersPool.set(letter, lettersPool.get(letter) + 1);
          } else {
              lettersPool.set(letter, 1);
          }
      }
  
      for(let letter of ransomNote) {
          if(lettersPool.has(letter)) {
              const rest = lettersPool.get(letter);
  
              if(rest === 0) return false;
  
              lettersPool.set(letter, lettersPool.get(letter) - 1);
          } else {
              return false;
          }
      }
      
      return true;
  };
  ```

### [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

- 排序比较

  ```javascript
  /**
   * @param {string} s
   * @param {string} t
   * @return {boolean}
   */
  var isAnagram = function(s, t) {
            if(s.length !== t.length) return false;
  
            const lettersOfS = s.split('').sort();
            const lettersOfT = t.split('').sort();
  
            for(let i = 0; i < lettersOfS.length; i++) {
              if(lettersOfS[i] !== lettersOfT[i]) return false;
            }
  
            return true;
          };
  ```

- 哈希映射

  这种方法我的以往题解写的比较多, 这里就不再写了.

### [将字符串拆分为若干长度为 k 的组](https://leetcode-cn.com/problems/divide-a-string-into-groups-of-size-k/)

- 模拟

  简单模拟操作

  ```javascript
  /**
   * @param {string} s
   * @param {number} k
   * @param {character} fill
   * @return {string[]}
   */
  var divideString = function(s, k, fill) {
            const result = [];
            const fillNumber = k - s.length % k;
  
            for(let i = k; i <= s.length; i += k) {
              result.push(s.slice(i - k, i));
            }
  
            if(fillNumber !== 0 && fillNumber !== k) result.push(s.slice(s.length - s.length % k) + Array(fillNumber).fill(fill).join(''));
  
            return result;
          };
  ```

### 小结

- 出现次数之类的题可以用哈希表来搞定
- 字符串和数组之间的关系十分紧密, 在需要时可以相互转化.

## 2022-02-17

### [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

- 虚拟头节点 + 指针

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} list1
   * @param {ListNode} list2
   * @return {ListNode}
   */
  var mergeTwoLists = function(list1, list2) {
    const dummyHead = new ListNode(0, list1);
    let current = dummyHead;
  
    while(list1 !== null && list2 !== null) {
      if(list1.val < list2.val) {
        current.next = list1;
        list1 = list1.next;
      } else {
        current.next = list2;
        list2 = list2.next;
      }
      current = current.next;
    }
  
    current.next = list1 === null? list2 : list1;
  
    return dummyHead.next;
  };
  ```

- 递归法

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} list1
   * @param {ListNode} list2
   * @return {ListNode}
   */
  var mergeTwoLists = function(list1, list2) {
      // 递归结束条件
      if(list1 === null) return list2;
      if(list2 === null) return list1;
  
      if(list1.val < list2.val) {
          list1.next = mergeTwoLists(list1.next, list2);
          return list1;
      } else {
          list2.next = mergeTwoLists(list1, list2.next);
          return list2;
      }
  };
  ```

### [移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

- 虚拟头节点

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @param {number} val
   * @return {ListNode}
   */
  var removeElements = function(head, val) {
    // 虚拟头节点
    const dummyHead = new ListNode(0, head);
    let current = dummyHead;
  
    while(current && current.next) {
      // 只需找到要删除的节点的上一个节点
      if(current.next.val === val) current.next = current.next.next;
      else current = current.next;
    }
  
    return dummyHead.next;
  };
  ```

### [删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

- 虚拟头节点 + 循环判断相等

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @return {ListNode}
   */
  var deleteDuplicates = function(head) {
      if(!(head && head.next)) return head;
  
      const dummyHead = new ListNode(0, head);
      let current = dummyHead;
  
      while(current.next && current.next.next) {
          if(current.next.val === current.next.next.val) {
              const val = current.next.val;
  
              while(current.next && current.next.val === val) {
                  current.next = current.next.next;
              }
          } else {
              current = current.next;
          }
      }
  
      return dummyHead.next;
  };
  ```

- 哈希表 + 两次遍历

## 2022-02-18 

### [删除链表中的结点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

- 直接修改值和下一个节点

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val) {
   *     this.val = val;
   *     this.next = null;
   * }
   */
  /**
   * @param {ListNode} node
   * @return {void} Do not return anything, modify node in-place instead.
   */
  var deleteNode = function(node) {
      node.val = node.next.val;
      node.next = node.next.next;
  };
  ```

### [删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

- 记录长度 + 两次遍历

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @param {number} n
   * @return {ListNode}
   */
  var removeNthFromEnd = function(head, n) {
    const dummyHead = new ListNode(0, head);
    let current = dummyHead;
    let length = 0;
    let index = 0;
  
    while(current.next) {
      length++;
      current = current.next;
    }
  
    current = dummyHead;
  
    while(length && current.next) {
      if(length - index === n) {
        current.next = current.next.next;
        return dummyHead.next;
      }
  
      current = current.next;
      index++;
    }
  };
  ```

- 虚拟头节点 + 快慢指针

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @param {number} n
   * @return {ListNode}
   */
  var removeNthFromEnd = function(head, n) {
      const dummyHead = new ListNode(0, head);
      // 分别定义 慢指针 和 快指针
      // 慢指针 指向元素以及左侧元素个数 + 快指针 指向元素以及右侧元素个数 = 链表长度
      let slow = dummyHead;
      let fast = dummyHead;
  
      // 将快指针移动 n 位
      for(let i = 0; i < n; i++) {
          fast = fast.next;
      }
  
      // 将 快指针 移动 链表长度 - n 位
      // 此时 慢指针指向倒数第 n + 1 位
      while(fast && fast.next) {
          fast = fast.next;
          slow = slow.next;
      }
  
      // 删除倒数第 n 位
      slow.next = slow.next.next;
  
      return dummyHead.next;
  };
  ```

### [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

- 双指针

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @return {ListNode}
   */
  var reverseList = function(head) {
    let resultHead = null;
    let currentOfHead = head;
  
    while(currentOfHead) {
      // 临时存储节点
      const node = resultHead;
      resultHead = currentOfHead;
      // 因为此时 resultHead 与 currentOfHead 指向的是同一节点, 因此需先改变 currentOfHead 指向
      currentOfHead = currentOfHead.next;
      resultHead.next = node;
    }
  
    return resultHead;
  };
  ```
  
- 递归

  ```javascript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @return {ListNode}
   */
  var reverseList = function(head) {
      if(!(head && head.next)) return head;
  
      let newHead = reverseList(head.next);
      head.next.next = head;
      head.next = null;
  
      return newHead;
  };
  ```
  
### 小结

- 删除链表中的节点有两种方式:

  - 将 目标节点 的 上一节点 的 下一节点 指向 目标节点 的 下一节点, 如 `pre.next = cur.next`
  - 直接修改 当前节点 的 值 为 当前节点 的 下一节点 的值, 修改 当前节点的 下一节点 为 当前节点 的 下一节点 的 下一节点, 如 `cur.val = cur.next.val; cur.next = cur.next.next`

- 删除倒数第 N 个节点有两种方式:
  
  - 记录长度并遍历
  - 使用双指针, 利用两指针之间的关系

- 反转链表的两种方式:

  - 前插入节点
  - 递归

## 2022-02-19 

### [反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

- 双指针

  ```javaScript
  /**
   * Definition for singly-linked list.
   * function ListNode(val, next) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.next = (next===undefined ? null : next)
   * }
   */
  /**
   * @param {ListNode} head
   * @param {number} left
   * @param {number} right
   * @return {ListNode}
   */
  var reverseBetween = function(head, left, right) {
      const dummyHead = new ListNode(0, head);
      let fast = dummyHead.next;
      let slow = dummyHead;
  
      for(let i = 0; i < left - 1; i++) {
          slow = slow.next;
          fast = fast.next;
      }
  
      const tempNode = slow.next;
  
      for(let i = left; i < right; i++) {
          const tempNode = slow.next;
          slow.next = fast.next;
          fast.next = fast.next.next;
          slow.next.next = tempNode;
      }
  
      tempNode.next = fast.next;
  
      return dummyHead.next;
  };
  ```

### [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

- 记录元素

  ```javaScript
  /**
   * Definition for singly-linked list.
   * function ListNode(val) {
   *     this.val = val;
   *     this.next = null;
   * }
   */
  
  /**
   * @param {ListNode} head
   * @return {boolean}
   */
  var hasCycle = function(head) {
      const itemList = [];
      let current = head;
  
      while(current) {
          if(itemList.includes(current)) return true;
          itemList.push(current);
          current = current.next;
      }
  
      return false;
  };
  ```
  
- 双指针

  ```javaScript
  /**
   * Definition for singly-linked list.
   * function ListNode(val) {
   *     this.val = val;
   *     this.next = null;
   * }
   */
  
  /**
   * @param {ListNode} head
   * @return {boolean}
   */
  var hasCycle = function(head) {
      if(head === null || head.next === null) return false;
  
      let fast = head;
      let slow = head;
  
      while(true) {
          if(fast === null || fast.next === null) return false;
  
          fast = fast.next.next;
          slow = slow.next;
  
          if(fast === slow) return true;
      }
  };
  ```

### [环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

- 记录元素

  ```javaScript
  /**
   * Definition for singly-linked list.
   * function ListNode(val) {
   *     this.val = val;
   *     this.next = null;
   * }
   */
  
  /**
   * @param {ListNode} head
   * @return {ListNode}
   */
  var detectCycle = function(head) {
      const itemList = [];
      let current = head;
  
      while(current) {
          if(itemList.includes(current)) return current;
          itemList.push(current);
          current = current.next;
      }
  
      return null;
  };
  ```

- 快慢指针

  ```javaScript
  /**
   * Definition for singly-linked list.
   * function ListNode(val) {
   *     this.val = val;
   *     this.next = null;
   * }
   */
  
  /**
   * @param {ListNode} head
   * @return {ListNode}
   */
  var detectCycle = function(head) {
      let fast = head;
      let slow = head;
  
      while(fast && fast.next) {
          fast = fast.next.next;
          slow = slow.next;
  
          if(fast === slow) {
              fast = head;
  
              while(fast !== slow) {
                  fast = fast.next;
                  slow = slow.next;
              }
  
              return fast;
          }
      }
  
      return null;
  };
  ```

## 2022-02-20 

### [二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)\

- 递归

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val, left, right) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.left = (left===undefined ? null : left)
   *     this.right = (right===undefined ? null : right)
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number[]}
   */
  var preorderTraversal = function(root) {
    const result = [];
    const DFS = (root) => {
      if(root) {
        result.push(root.val);
        DFS(root.left);
        DFS(root.right);
      } else return;
    }
  
    DFS(root);
  
    return result;
  };
  ```

- 栈保存根节点再展开

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val, left, right) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.left = (left===undefined ? null : left)
   *     this.right = (right===undefined ? null : right)
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number[]}
   */
  var preorderTraversal = function(root) {
      const result = [];
      const stack = [];
  
      if(root) {
          stack.push(root);
  
          while(stack.length) {
              const current = stack.pop();
              result.push(current.val);
              
              if(current.right) stack.push(current.right);
              if(current.left) stack.push(current.left);
          }
      }
  
      return result;
  };
  ```

### [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

- 递归
- 栈保存根节点再展开

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val, left, right) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.left = (left===undefined ? null : left)
   *     this.right = (right===undefined ? null : right)
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number[]}
   */
  var inorderTraversal = function(root) {
      const result = [];
      const stack = [];
  
      while(stack.length || root) {
          while(root) {
              stack.push(root);
              root = root.left;
          }
  
          root = stack.pop();
  
          result.push(root.val);
  
          root = root.right;
      }
      
      return result;
  };
  ```

### [二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

- 递归
- 栈记录元素再展开

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val, left, right) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.left = (left===undefined ? null : left)
   *     this.right = (right===undefined ? null : right)
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number[]}
   */
  var postorderTraversal = function(root) {
      if(!root) return [];
  
      const result = [];
      const stack = [];
  
      stack.push(root);
  
      while(stack.length) {
          const current = stack.pop();
  
          result.unshift(current.val);
  
          if(current.left) stack.push(current.left);
          if(current.right) stack.push(current.right);
      }
  
      return result;
  };
  ```

## 2022-02-21

### [二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

- 队列保存每层节点

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val, left, right) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.left = (left===undefined ? null : left)
   *     this.right = (right===undefined ? null : right)
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number[][]}
   */
  var levelOrder = function(root) {
      if(!root) return [];
  
      const result = [];
      const queue = [];
  
      queue.push(root);
  
      while(queue.length) {
          const floor = [];
          const length = queue.length;
          for(let i = 0; i < length; i++) {
              const current = queue.shift();
  
              floor.push(current.val);
              
              if(current.left) queue.push(current.left);
              if(current.right) queue.push(current.right);
          }
  
          result.push(floor);
      }
      
      return result;
  };
  ```

### [从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

- 队列保存每层节点

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val) {
   *     this.val = val;
   *     this.left = this.right = null;
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number[]}
   */
  var levelOrder = function(root) {
      if(!root) return [];
  
      const result = [];
      const queue = [];
  
      queue.push(root);
  
      while(queue.length) {
          const length = queue.length;
  
          for(let i = 0; i < queue.length; i++) {
              const current = queue.shift();
  
              result.push(current.val);
  
              if(current.left) queue.push(current.left);
              if(current.right) queue.push(current.right);
          }
      }
  
      return result;
  };
  ```
  
### [二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

- dfs 向上传递深度

  ```javaScript
  /**
   * Definition for a binary tree node.
   * function TreeNode(val, left, right) {
   *     this.val = (val===undefined ? 0 : val)
   *     this.left = (left===undefined ? null : left)
   *     this.right = (right===undefined ? null : right)
   * }
   */
  /**
   * @param {TreeNode} root
   * @return {number}
   */
  var maxDepth = function(root) {
    const dfs = (root) => {
      if(!root) return 0;
  
      let leftDepth = dfs(root.left);
      let rightDepth = dfs(root.right);
  
      return Math.max(leftDepth, rightDepth) + 1;
    }
  
    return dfs(root);
  };
  ```
