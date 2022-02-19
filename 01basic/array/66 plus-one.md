<!--
 * @Author: liwenyue
 * @Date: 2022-02-19 11:40:36
 * @LastEditors: liwenyue
 * @LastEditTime: 2022-02-19 12:37:40
-->

# 题目：https://leetcode-cn.com/problems/plus-one/

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1：

输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
示例 2：

输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
示例 3：

输入：digits = [0]
输出：[1]

**提示**：

1 <= digits.length <= 100
0 <= digits[i] <= 9

思考：

1. 根据题目给出的条件数组单个元素只存放单个数字，由提示也可得知
   1. 有进位的可能
2. 数组长度小于等于 100
   1. 意味着数组转换为数字，整形有可能会溢出
3. 整数不会以零开头，除了整数 0 以外的
   1. 可以转数字
4. 最高位数组在首位，意味着
   1. 可以转数字
5. 第一种情况，末位无进位，最后一位直接相加即可, 如 27 > 28
6. 第二种情况，末位有进位，进位在中间停止, 如 499 > 500
7. 第三种情况，末位有进位，进位直到最前面的一位，需要扩充数组,如 999 > 1000

思路：

1. 写一个进位标志位 carry
2. 首先对最后一位相加
3. 循环遍历数组，倒序遍历
   1. 如果此时 carry 为 true，说明上一位有进位，此时需要对当前位+1
   2. 对当前位加一之后再次判断 carry 值，如果 > 9, 就说明有进位
   3. 对当前位赋值，直接 对 10 取余即可
4. 循环数组结束之后，如果 carry 仍为 true，说明走到了第三种情况下，此时直接在数组最前方添加 1 即可

```js
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function (digits) {
  let length = digits.length,
    carry = false;
  digits[length - 1]++;
  for (let i = length - 1; i >= 0; i--) {
    if (carry) digits[i]++;
    carry = digits[i] > 9;
    digits[i] %= 10;
  }
  if (carry) digits.unshift(1);
  return digits;
};
console.log(plusOne([9, 9, 9]));
```

思路 2

1. 标志位
2. 直接 while 循环
3. carry 值的判断以 取 10 的商数
4. 后续逻辑同上

```js
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function (digits) {
  let carry = 1,
    sum = 0,
    index = digits.length - 1;

  while (carry > 0 && index > -1) {
    sum = digits[index] + 1;
    carry = Math.floor(sum / 10);
    digits[index] = sum % 10;
    index--;
  }

  carry && digits.unshift(carry);

  return digits;
};

console.log(plusOne([4, 9, 9]));
```

思路 3

1. 不用标志位，直接在循环里做判断，如果参数的当前位是 0，直接进下一个循环
2. 否则直接 return 此数组

```js
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function (digits) {
  const len = digits.length;
  for (let i = len - 1; i >= 0; i--) {
    digits[i]++;
    digits[i] %= 10;
    if (digits[i] != 0) return digits;
  }
  digits = [...Array(len + 1)].map((_) => 0);
  digits[0] = 1;
  return digits;
};
console.log(plusOne([1, 9, 9]));
```
