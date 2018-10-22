# 阶乘

阶乘的形式非常简单

```text
5! = 5 * 4 * 3 * 2 * 1 = 120
```

| n | n! |
| :--- | :--- |
| 0 | 1 |
| 1 | 1 |
| 2 | 2 |
| 3 | 6 |
| 4 | 24 |
| 5 | 120 |
| 6 | 720 |

### 一般方法

使用for循环进行遍历求解

```javascript
/**
 * @param {number} number
 * @return {number}
 */
export default function factorial(number) {
  let result = 1;

  for (let i = 2; i <= number; i += 1) {
    result *= i;
  }

  return result;
}
```

### 递归方法

```javascript
/**
 * @param {number} number
 * @return {number}
 */
export default function factorialRecursive(number) {
  return number > 1 ? number * factorialRecursive(number - 1) : 1;
}
```



