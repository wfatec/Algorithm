# 判断2的次方

### 传统方法

将数值不断除以2，直到该值变为1，并判断余数是否始终为零，若不满足该条件，则该数不为2的次方数。

方法如下：

```javascript
/**
 * @param {number} number
 * @return {boolean}
 */
export default function isPowerOfTwo(number) {
  // 1 (2^0) is the smallest power of two.
  if (number < 1) {
    return false;
  }

  // Let's find out if we can divide the number by two
  // many times without remainder.
  let dividedNumber = number;
  while (dividedNumber !== 1) {
    if (dividedNumber % 2 !== 0) {
      // For every case when remainder isn't zero we can say that this number
      // couldn't be a result of power of two.
      return false;
    }

    dividedNumber /= 2;
  }

  return true;
}
```

### Bit求解

将2的次方转化为二进制时，可以发现一个有趣的现象，即始终只有一位是1。只有一种情况例外，即有符号整数\(例如一个8位的有符号整数-128看起来像这样：`10000000`\)

```text
1: 0001
2: 0010
4: 0100
8: 1000
```

因此，在在确定一个数大于0时，我们可以使用Bit方法来测试是否有且只有一位置1。

```text
number & (number - 1)
```

例如数字8，操作过程将看起来像这样：

```text
  1000
- 0001
  ----
  0111
  
  1000
& 0111
  ----
  0000
```

最终实现方法为：

```javascript
/**
 * @param {number} number
 * @return {boolean}
 */
export default function isPowerOfTwoBitwise(number) {
  // 1 (2^0) is the smallest power of two.
  if (number < 1) {
    return false;
  }

  /*
   * Powers of two in binary look like this:
   * 1: 0001
   * 2: 0010
   * 4: 0100
   * 8: 1000
   *
   * Note that there is always exactly 1 bit set. The only exception is with a signed integer.
   * e.g. An 8-bit signed integer with a value of -128 looks like:
   * 10000000
   *
   * So after checking that the number is greater than zero, we can use a clever little bit
   * hack to test that one and only one bit is set.
   */
  return (number & (number - 1)) === 0;
}
```

**注意：Bit方法往往具有更高的执行效率！**

