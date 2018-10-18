# Fibonacci数列

在数学范畴内，斐波那契数列的定义为每一位的值为前面两位数值的和，即：

```javascript
// F(N) = F(N-1) + F(N-2)  N为自然数
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...
```

下面这张图中每个正方形的边长即构成一个斐波那契数列： ![img](https://camo.githubusercontent.com/f653fca3a6fcf1733d0b19c3ddb37622926b42e7/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f642f64622f333425324132312d4669626f6e61636369426c6f636b732e706e67)

此外，通过斐波那契数列可以得到近似黄金分割的曲线：

![img](https://camo.githubusercontent.com/e1127247ec2da22f21e548352a86e7180f10d7bf/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f322f32652f4669626f6e6163636953706972616c2e737667)

## 返回一个斐波那契数列

```javascript
/**
 * 返回一个斐波那契数列.
 *
 * @param n
 * @return {number[]}
 */
export default function fibonacci(n) {
    const fibSequence = [1];

    let currentValue = 1;
    let previousValue = 0;

    if (n === 1) {
      return fibSequence;
    }

    let iterationsCounter = n - 1;

    while (iterationsCounter) {
      currentValue += previousValue;
      previousValue = currentValue - previousValue;

      fibSequence.push(currentValue);

      iterationsCounter -= 1;
    }

    return fibSequence;
}
```

## 计算特定位置的斐波那契数列的值

### 基础版本

直接使用递归调用，缺点是内存和上下文消耗较大。

```javascript
/**
 * 递归方式计算特定位置的斐波那契数列的值
 *
 * @param n
 * @return {number}
 */
export default function fibonacciNth(n) {
    if (n <= 0) {
        return 0;
    } else if (n === 1) {
        return 1;
    }
    return fibo(n - 1) + fibo(n - 2)
}
```

### 尾递归

使用尾递归，取消过多的堆栈记录，而只记录最外层和当前层的信息，完成计算后，立刻返回到最上层。这也就不会有栈溢出的问题，同时减少了内存以及上下文切换的损耗。 **尾递归的本质实际上就是将方法需要的上下文通过方法的参数传递进下一次调用之中**，以达到去除上层依赖。\([参考](http://imweb.io/topic/584d33049be501ba17b10aaf)\)

```javascript
/**
 * 使用尾递归方式计算特定位置的斐波那契数列的值
 *
 * @param n 
 * @param num1
 * @param num2
 * @return {number}
 */
export default function fibonacciNth(n, num1 = 1, num2 = 1) {
    if (n <= 0) {
        return 0;
    } else if (n === 1) {
        return num1;
    }
    return fibonacciNth(n - 1, num2, num1 + num2);
}
```

### 动态编程

本质上将求解斐波那契数值的过程置于循环内部，用更直接的方式来求解，有时候简单直接蚕食效率最高的。

```javascript
/**
 * 使用动态编程方式计算特定位置的斐波那契数列的值
 *
 * @param n
 * @return {number}
 */
export default function fibonacciNth(n) {
    let currentValue = 1;
    let previousValue = 0;

    if (n === 1) {
      return 1;
    }

    let iterationsCounter = n - 1;

    while (iterationsCounter) {
      currentValue += previousValue;
      previousValue = currentValue - previousValue;

      iterationsCounter -= 1;
    }

    return currentValue;
}
```

### 使用封闭解

关于fibonacci封闭解的概念可以参考[这里](https://en.wikipedia.org/wiki/Fibonacci_number#Closed-form_expression)

我们可以通过解析形式来用数学方法计算特定位置的斐波那契数列的值。

```javascript
/**
 * Calculate fibonacci number at specific position using closed form function (Binet's formula).
 * @see: https://en.wikipedia.org/wiki/Fibonacci_number#Closed-form_expression
 *
 * @param {number} position - Position number of fibonacci sequence (must be number from 1 to 75).
 * @return {number}
 */
export default function fibonacciClosedForm(position) {
  const topMaxValidPosition = 75;

  // Check that position is valid.
  if (position < 1 || position > topMaxValidPosition) {
    throw new Error(`Can't handle position smaller than 1 or greater than ${topMaxValidPosition}`);
  }

  // Calculate √5 to re-use it in further formulas.
  const sqrt5 = Math.sqrt(5);
  // Calculate φ constant (≈ 1.61803).
  const phi = (1 + sqrt5) / 2;

  // Calculate fibonacci number using Binet's formula.
  return Math.floor((phi ** position) / sqrt5 + 0.5);
}
```

使用封闭解我们甚至可以根据具体的值，判断该值在fibonacci数列中的位置。

