# 素数检测

[素数](https://en.wikipedia.org/wiki/Prime_number)是指只能被1和本身整除，且大于1的自然数，素数的检测即判断输入值是否为素数，可参考[维基百科素数检测](https://en.wikipedia.org/wiki/Primality_test)。

```javascript
/**
 * @param {number} number
 * @return {boolean}
 */
export default function trialDivision(number) {
  // Check if number is integer.
  if (number % 1 !== 0) {
    return false;
  }

  if (number <= 1) {
    // If number is less than one then it isn't prime by definition.
    return false;
  }

  if (number <= 3) {
    // All numbers from 2 to 3 are prime.
    return true;
  }

  // If the number is not divided by 2 then we may eliminate all further even dividers.
  if (number % 2 === 0) {
    return false;
  }

  // If there is no dividers up to square root of n then there is no higher dividers as well.
  const dividerLimit = Math.sqrt(number);
  for (let divider = 3; divider <= dividerLimit; divider += 2) {
    if (number % divider === 0) {
      return false;
    }
  }

  return true;
}
```

### 利用正则表达式

检测素数的正则表达式参考了[检查素数的正则表达式](https://coolshell.cn/articles/2704.html) 以及 [Solving Algebraic Equations Using Regular Expressions](http://blog.stevenlevithan.com/archives/algebra-with-regexes)：

```text
/^1?$|^(11+?)\1+$/
```

* **第一部分：/^1?$/**， 这个部分表示匹配“空串”以及字串中只有一个“1”的字符串。
* **第二部分：/^\(11+?\)\1+$/**，这个部分是整个表达式的关键部分。其可以分成两个部分，**\(11+?\)** 和**\1+$**，前半部很简单了，匹配以“11”开头的并重复0或n个1的字符串，后面的部分意思是把前半部分作为一个字串去匹配还剩下的字符串1次或多次（这句话的意思是——剩余的字串的1的个数要是前面字串1个数的整数倍）。

 可见这个正规则表达式是取非素数，要得到素数还得要对整个表达式求反。

**示例一：判断自然数8**。我们可以知道，8转成我们的格式就是“11111111”，对于**\(11+?\)**，其匹配了“11”，于是还剩下“111111”，而**\1+$**正好匹配了剩下的“111111”，因为，“11”这个模式在“111111”出现了三次，符合模式匹配，返回true。所以，匹配成功，于是这个数不是质数。

**示例二：判断自然数11**。转成我们需要的格式是“11111111111”（十一个1），对于**\(11+?\)**，其匹配了“11”（前两个1），还剩下“111111111”（九个1），而**\1+$**无法为“11”匹配那“九个1”，因为“11”这个模式并没有在“九个1”这个串中正好出现N次。于是，我们的正则表达式引擎会尝试下一种方法，先匹配“111”（前三个1），然后把“111”作为模式去匹配剩下的“11111111”（八个1），很明显，那“八个1”并没有匹配“三个1”多次。所以，引擎会继续向下尝试……直至尝试所有可能都无法匹配成功。所以11是素数。

通过示例二，我们可以得到这样的等价数算算法，正则表达式会匹配这若干个1中有没有出现“二个1”的整数倍，“三个1”的整数倍，“四个1”的整数倍……，而，这正好是我们需要的算素数的算法。现在大家明白了吧。

根据[js 正则之 检测素数](https://www.cnblogs.com/52cik/p/js-regular-prime.html) 中所述，我们可以不必要检测 0 1 之类的，因为根据素数的解释，素数是从 2 开始的。所以只要加一个判断条件 n &lt; 2 的都不是素数，其他的则用 /^\(11+?\)\1+$/ 进行验证即可，  
不仅提升了效率，而且防止传入负数而报错。

最终实现为：

```javascript
/**
 * 判断是否是素数
 * @param  {Number} n  要判断是数字
 * @return {Boolean}   返回 true|false
 */
export default function trialDivision(number) {
  return number < 2 ? false : !/^(11+?)\1+$/.test(Array(number + 1).join('1'));
}
```

若想获取某个范围内的所有素数，可以这样：

```javascript
/**
 * 遍历素数
 * @param  {Number} max 遍历 2-max 之间的素数
 * @return {Array}      返回 2-max 之间所有素数
 */
function prime(max) {
    var re = new RegExp('^(11+?)\\1+$'), // 检测质数正则
        str = '1', // 根据当前 i 生成对应个数 1 
        i = 1, // 计数器
        ret = []; // 质数结果集
    while (max > i++)
        re.test(str += '1') || ret.push(i);
    return ret;
}
var arr = prime(100);
console.log(arr);
```

