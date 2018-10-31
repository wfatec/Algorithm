# 筛选素数

在数学领域，[素数筛\(Sieve of Eratosthenes\) ](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)是指找出给定限值内的所有素数。

### 过程简介

筛选给定值n内所有素数的方法简单包括如下步骤：

1. 创建一个从2到n的整数序列\(2,3,4,...,n\).
2. 初始化，将p设为2，即最小的素数.
3. 从2开始，依次将p乘以整数\(2p,3p,4p,...\)，之道乘积大于n，标记所有乘积，但p本身不被标记.
4. 找出未标记的大于p的第一个数，若没有这样数，则停止，否则将p赋值为该值，然后重复过程3.
5. 最终剩余未标记的值即为所有所有素数.

事实上，我们也可以改进过程3，直接从p\*p开始标记，因为p\*p以内的数值必然已经在之前被标记过了。因此我们可以在`p*p>n`时，跳出循环。

### 最终实现

```javascript
/**
 * @param {number} maxNumber
 * @return {number[]}
 */
export default function sieveOfEratosthenes(maxNumber) {
  const isPrime = new Array(maxNumber + 1).fill(true);
  isPrime[0] = false;
  isPrime[1] = false;

  const primes = [];

  for (let number = 2; number <= maxNumber; number += 1) {
    if (isPrime[number] === true) {
      primes.push(number);

      /*
       * Optimisation.
       * Start marking multiples of `p` from `p * p`, and not from `2 * p`.
       * The reason why this works is because, at that point, smaller multiples
       * of `p` will have already been marked `false`.
       *
       * Warning: When working with really big numbers, the following line may cause overflow
       * In that case, it can be changed to:
       * let nextNumber = 2 * number;
       */
      let nextNumber = number * number;

      while (nextNumber <= maxNumber) {
        isPrime[nextNumber] = false;
        nextNumber += number;
      }
    }
  }

  return primes;
}
```

