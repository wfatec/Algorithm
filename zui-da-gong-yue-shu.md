# 最大公约数

在数学领域，[欧几里得算法](https://baike.baidu.com/item/%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%B7%E7%AE%97%E6%B3%95/9002848?fromtitle=%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E7%AE%97%E6%B3%95&fromid=1647675)是计算最大公约数（GCD）最为高效的方法。

 欧几里德算法又称[辗转相除法](https://baike.baidu.com/item/%E8%BE%97%E8%BD%AC%E7%9B%B8%E9%99%A4%E6%B3%95/4625352)，是指用于计算两个[正整数](https://baike.baidu.com/item/%E6%AD%A3%E6%95%B4%E6%95%B0/8461335)a，b的[最大公约数](https://baike.baidu.com/item/%E6%9C%80%E5%A4%A7%E5%85%AC%E7%BA%A6%E6%95%B0/869308)。应用领域有数学和计算机两个方面。计算公式`gcd(a,b) = gcd(b,a mod b)`。

举个例子，`21`是`252`和`105`的最大公约数，那么`21`也同样是`105`和`252-105=147`的最大公约数。这样的替换，去除了两数最大的公共部分,，直到左右两数相等，则该等值即为最大公约数。当然，也可以使用两数求余的方式进行迭代计算，直到右侧参数值为0.

将此过程反过来看，GCD可以表示为两个原始数分别乘以一个整数（正负均可）后的和。例如`21=5 * 105 + (-2) * 252`。

### 递归解法

```javascript
/**
 * Recursive version of Euclidean Algorithm of finding greatest common divisor (GCD).
 * @param {number} originalA
 * @param {number} originalB
 * @return {number}
 */
export default function euclideanAlgorithm(originalA, originalB) {
  // Make input numbers positive.
  const a = Math.abs(originalA);
  const b = Math.abs(originalB);

  // To make algorithm work faster instead of subtracting one number from the other
  // we may use modulo operation.
  return (b === 0) ? a : euclideanAlgorithm(b, a % b);
}
```

### 迭代解法

```javascript
/**
 * Iterative version of Euclidean Algorithm of finding greatest common divisor (GCD).
 * @param {number} originalA
 * @param {number} originalB
 * @return {number}
 */
export default function euclideanAlgorithmIterative(originalA, originalB) {
  // Make input numbers positive.
  let a = Math.abs(originalA);
  let b = Math.abs(originalB);

  // Subtract one number from another until both numbers would become the same.
  // This will be out GCD. Also quit the loop if one of the numbers is zero.
  while (a && b && a !== b) {
    [a, b] = a > b ? [a - b, b] : [a, b - a];
  }

  // Return the number that is not equal to zero since the last subtraction (it will be a GCD).
  return a || b;
}
```

