<!--
 * @Author: sherlyzz
 * @Date: 2022-02-10
 * @LastEditTime: 2022-02-10
 * @LastEditors: sherlyzz
 * @Description: LeetCode 每日刷题记录
-->

# LeetCode 每日

LeetCode 的每日刷题记录, 记录题解以及思路.

## 2020-02-10

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
