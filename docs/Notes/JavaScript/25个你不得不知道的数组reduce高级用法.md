---
title: "25个你不得不知道的数组reduce高级用法"
date: "2022-6-13"
tags: ["javascript"]
---

转载：[25 个你不得不知道的数组 reduce 高级用法]([25 个你不得不知道的数组 reduce 高级用法 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904063729926152))

`reduce`作为 ES5 新增的常规数组方法之一，对比`forEach`、`filter`和`map`，在实际使用上好像有些被忽略，发现身边的人极少使用它，导致这个如此强大的方法被逐渐埋没。

如果经常使用`reduce`，怎么可能放过如此好用的它呢！我还是得把他从尘土中取出来擦干净，奉上它的高级用法给大家。一个如此好用的方法不应该被大众埋没。

下面对`reduce`的语法进行简单说明，详情可查看`MDN`的[reduce()](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FArray%2Freduce)的相关说明。

- 定义：对数组中的每个元素执行一个自定义的累计器，将其结果汇总为单个返回值
- 形式：`array.reduce((t, v, i, a) => {}, initValue)`
- 参数
  - **callback**：回调函数(`必选`)
  - **initValue**：初始值(`可选`)
- 回调函数的参数
  - **total**(`t`)：累计器完成计算的返回值(`必选`)
  - **value**(`v`)：当前元素(`必选`)
  - **index**(`i`)：当前元素的索引(`可选`)
  - **array**(`a`)：当前元素所属的数组对象(`可选`)
- 过程
  - 以`t`作为累计结果的初始值，不设置`t`则以数组第一个元素为初始值
  - 开始遍历，使用累计器处理`v`，将`v`的映射结果累计到`t`上，结束此次循环，返回`t`
  - 进入下一次循环，重复上述操作，直至数组最后一个元素
  - 结束遍历，返回最终的`t`

`reduce`的精华所在是将累计器逐个作用于数组成员上，**把上一次输出的值作为下一次输入的值**。下面举个简单的栗子，看看`reduce`的计算结果。

```js
const arr = [3, 5, 1, 4, 2];
const a = arr.reduce((t, v) => t + v);
// 等同于
const b = arr.reduce((t, v) => t + v, 0);
```

代码不太明白没关系，贴一个`reduce`的作用动图应该就会明白了。

![reduce](http://qny.mrpwei.cc/uPic/7ee0cc54777d42498fa32778bd78e6d5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

`reduce`实质上是一个累计器函数，通过用户自定义的累计器对数组成员进行自定义累计，得出一个由累计器生成的值。另外`reduce`还有一个胞弟`reduceRight`，两个方法的功能其实是一样的，只不过`reduce`是升序执行，`reduceRight`是降序执行。

> 对空数组调用 reduce()和 reduceRight()是不会执行其回调函数的，可认为 reduce()对空数组无效

### 高级用法

单凭以上一个简单栗子不足以说明`reduce`是个什么。为了展示`reduce`的魅力，我为大家提供 25 种场景来应用`reduce`的高级用法。有部分高级用法可能需要结合其他方法来实现，这样为`reduce`的多元化提供了更多的可能性。

> 部分示例代码的写法可能有些骚，看得不习惯可自行整理成自己的习惯写法

##### 累加累乘

```js
function Accumulation(...vals) {
  return vals.reduce((t, v) => t + v, 0);
}

function Multiplication(...vals) {
  return vals.reduce((t, v) => t * v, 1);
}

Accumulation(1, 2, 3, 4, 5); // 15
Multiplication(1, 2, 3, 4, 5); // 120
```

##### 权重求和

```js
const scores = [
  { score: 90, subject: "chinese", weight: 0.5 },
  { score: 95, subject: "math", weight: 0.3 },
  { score: 85, subject: "english", weight: 0.2 },
];

const result = scores.reduce((t, v) => t + v.score * v.weight, 0); // 90.5
```

##### 代替 reverse

```js
function Reverse(arr = []) {
  return arr.reduceRight((t, v) => (t.push(v), t), []);
}

Reverse([1, 2, 3, 4, 5]); // [5, 4, 3, 2, 1]
```

##### 代替 map 和 filter

```js
const arr = [0, 1, 2, 3];

// 代替map：[0, 2, 4, 6]
const a = arr.map((v) => v * 2);
const b = arr.reduce((t, v) => [...t, v * 2], []);

// 代替filter：[2, 3]
const c = arr.filter((v) => v > 1);
const d = arr.reduce((t, v) => (v > 1 ? [...t, v] : t), []);

// 代替map和filter：[4, 6]
const e = arr.map((v) => v * 2).filter((v) => v > 2);
const f = arr.reduce((t, v) => (v * 2 > 2 ? [...t, v * 2] : t), []);
```

##### 代替 some 和 every

```js
const scores = [
  { score: 45, subject: "chinese" },
  { score: 90, subject: "math" },
  { score: 60, subject: "english" },
];

// 代替some：至少一门合格
const isAtLeastOneQualified = scores.reduce(
  (t, v) => t || v.score >= 60,
  false
); // true

// 代替every：全部合格
const isAllQualified = scores.reduce((t, v) => t && v.score >= 60, true); // false
```

##### 数组分割

```js
function Chunk(arr = [], size = 1) {
  return arr.length
    ? arr.reduce(
        (t, v) => (
          t[t.length - 1].length === size
            ? t.push([v])
            : t[t.length - 1].push(v),
          t
        ),
        [[]]
      )
    : [];
}

const arr = [1, 2, 3, 4, 5];
Chunk(arr, 2); // [[1, 2], [3, 4], [5]]
```

##### 数组过滤

```js
function Difference(arr = [], oarr = []) {
  return arr.reduce((t, v) => (!oarr.includes(v) && t.push(v), t), []);
}

const arr1 = [1, 2, 3, 4, 5];
const arr2 = [2, 3, 6];
Difference(arr1, arr2); // [1, 4, 5]
```

##### 数组填充

```js
function Fill(arr = [], val = "", start = 0, end = arr.length) {
  if (start < 0 || start >= end || end > arr.length) return arr;
  return [
    ...arr.slice(0, start),
    ...arr.slice(start, end).reduce((t, v) => (t.push(val || v), t), []),
    ...arr.slice(end, arr.length),
  ];
}

const arr = [0, 1, 2, 3, 4, 5, 6];
Fill(arr, "aaa", 2, 5); // [0, 1, "aaa", "aaa", "aaa", 5, 6]
```

##### 数组扁平 💡

```js
function Flat(arr = []) {
  return arr.reduce((t, v) => t.concat(Array.isArray(v) ? Flat(v) : v), []);
}

const arr = [0, 1, [2, 3], [4, 5, [6, 7]], [8, [9, 10, [11, 12]]]];
Flat(arr); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```

##### 数组去重

```js
function Uniq(arr = []) {
  return arr.reduce((t, v) => (t.includes(v) ? t : [...t, v]), []);
}

const arr = [2, 1, 0, 3, 2, 1, 2];
Uniq(arr); // [2, 1, 0, 3]
```

##### 数组最大最小值

```js
function Max(arr = []) {
  return arr.reduce((t, v) => (t > v ? t : v));
}

function Min(arr = []) {
  return arr.reduce((t, v) => (t < v ? t : v));
}

const arr = [12, 45, 21, 65, 38, 76, 108, 43];
Max(arr); // 108
Min(arr); // 12
```

##### 数组成员独立拆解

```js
function Unzip(arr = []) {
  return arr.reduce(
    (t, v) => (v.forEach((w, i) => t[i].push(w)), t),
    Array.from({ length: Math.max(...arr.map((v) => v.length)) }).map((v) => [])
  );
}

const arr = [
  ["a", 1, true],
  ["b", 2, false],
];
Unzip(arr); // [["a", "b"], [1, 2], [true, false]]
```

##### 数组成员个数统计

```js
function Count(arr = []) {
  return arr.reduce((t, v) => ((t[v] = (t[v] || 0) + 1), t), {});
}

const arr = [0, 1, 1, 2, 2, 2];
Count(arr); // { 0: 1, 1: 2, 2: 3 }
```

此方法是字符统计和单词统计的原理，入参时把字符串处理成数组即可。

##### 数组成员位置记录

```js
function Position(arr = [], val) {
  return arr.reduce((t, v, i) => (v === val && t.push(i), t), []);
}

const arr = [2, 1, 5, 4, 2, 1, 6, 6, 7];
Position(arr, 2); // [0, 4]
```

##### 数组成员特性分组

```js
function Group(arr = [], key) {
  return key
    ? arr.reduce(
        (t, v) => (!t[v[key]] && (t[v[key]] = []), t[v[key]].push(v), t),
        {}
      )
    : {};
}

const arr = [
  { area: "GZ", name: "YZW", age: 27 },
  { area: "GZ", name: "TYJ", age: 25 },
  { area: "SZ", name: "AAA", age: 23 },
  { area: "FS", name: "BBB", age: 21 },
  { area: "SZ", name: "CCC", age: 19 },
]; // 以地区area作为分组依据
Group(arr, "area"); // { GZ: Array(2), SZ: Array(2), FS: Array(1) }
```

##### 数组成员所含关键字统计

```js
function Keyword(arr = [], keys = []) {
  return keys.reduce(
    (t, v) => (arr.some((w) => w.includes(v)) && t.push(v), t),
    []
  );
}

const text = [
  "今天天气真好，我想出去钓鱼",
  "我一边看电视，一边写作业",
  "小明喜欢同桌的小红，又喜欢后桌的小君，真TM花心",
  "最近上班喜欢摸鱼的人实在太多了，代码不好好写，在想入非非",
];
const keyword = ["偷懒", "喜欢", "睡觉", "摸鱼", "真好", "一边", "明天"];
Keyword(text, keyword); // ["喜欢", "摸鱼", "真好", "一边"]
```

##### 字符串翻转

```js
function ReverseStr(str = "") {
  return str.split("").reduceRight((t, v) => t + v);
}

const str = "reduce最牛逼";
ReverseStr(str); // "逼牛最ecuder"
```

##### 数字千分化

```js
function ThousandNum(num = 0) {
  const str = (+num).toString().split(".");
  const int = (nums) =>
    nums
      .split("")
      .reverse()
      .reduceRight((t, v, i) => t + (i % 3 ? v : `${v},`), "")
      .replace(/^,|,$/g, "");
  const dec = (nums) =>
    nums
      .split("")
      .reduce((t, v, i) => t + ((i + 1) % 3 ? v : `${v},`), "")
      .replace(/^,|,$/g, "");
  return str.length > 1 ? `${int(str[0])}.${dec(str[1])}` : int(str[0]);
}

ThousandNum(1234); // "1,234"
ThousandNum(1234.0); // "1,234"
ThousandNum(0.1234); // "0.123,4"
ThousandNum(1234.5678); // "1,234.567,8"
```

##### 异步累计

```js
async function AsyncTotal(arr = []) {
  return arr.reduce(async (t, v) => {
    const at = await t;
    const todo = await Todo(v);
    at[v] = todo;
    return at;
  }, Promise.resolve({}));
}

const result = await AsyncTotal(); // 需要在async包围下使用
```

##### 斐波那契数列

```js
function Fibonacci(len = 2) {
  const arr = [...new Array(len).keys()];
  return arr.reduce(
    (t, v, i) => (i > 1 && t.push(t[i - 1] + t[i - 2]), t),
    [0, 1]
  );
}

Fibonacci(10); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

##### URL 参数反序列化

```js
function ParseUrlSearch() {
  return location.search
    .replace(/(^\?)|(&$)/g, "")
    .split("&")
    .reduce((t, v) => {
      const [key, val] = v.split("=");
      t[key] = decodeURIComponent(val);
      return t;
    }, {});
}

// 假设URL为：https://www.baidu.com?age=25&name=TYJ
ParseUrlSearch(); // { age: "25", name: "TYJ" }
```

##### URL 参数序列化

```js
function StringifyUrlSearch(search = {}) {
  return Object.entries(search)
    .reduce(
      (t, v) => `${t}${v[0]}=${encodeURIComponent(v[1])}&`,
      Object.keys(search).length ? "?" : ""
    )
    .replace(/&$/, "");
}

StringifyUrlSearch({ age: 27, name: "YZW" }); // "?age=27&name=YZW"
```

##### 返回对象指定键值

```js
function GetKeys(obj = {}, keys = []) {
  return Object.keys(obj).reduce(
    (t, v) => (keys.includes(v) && (t[v] = obj[v]), t),
    {}
  );
}

const target = { a: 1, b: 2, c: 3, d: 4 };
const keyword = ["a", "d"];
GetKeys(target, keyword); // { a: 1, d: 4 }
```

##### 数组转对象

```js
const people = [
  { area: "GZ", name: "YZW", age: 27 },
  { area: "SZ", name: "TYJ", age: 25 },
];
const map = people.reduce((t, v) => {
  const { name, ...rest } = v;
  t[name] = rest;
  return t;
}, {}); // { YZW: {…}, TYJ: {…} }
```

##### Redux Compose 函数原理

```js
function Compose(...funs) {
  if (funs.length === 0) {
    return (arg) => arg;
  }
  if (funs.length === 1) {
    return funs[0];
  }
  return funs.reduce(
    (t, v) =>
      (...arg) =>
        t(v(...arg))
  );
}
```
