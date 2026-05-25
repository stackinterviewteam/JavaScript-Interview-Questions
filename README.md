---
title: "Tricky JavaScript Code Snippets Questions Asked in the Interview 2026 (ES6/ES7/ES8/ES9)"
description: "Sharpen your JavaScript skills with these tricky 2026 interview code snippets. Covering closures, hoisting, async patterns, destructuring, generators, and modern ES6+ features with detailed explanations."
githubPath: "https://github.com/stackinterviewteam/JavaScript-Interview-Questions"
---

<span style=" font-size: 1rem; border-bottom: 1px solid grey;"> Updated May 25, 2026 </span>

In this article, we cover a curated set of tricky JavaScript interview questions for 2026 — including output-guessing challenges, coding problems, and MCQs based on modern ES6+ syntax.

Whether you're targeting a FAANG-level role or a fast-growing startup, understanding these nuances will help you stand out. We cover closures, `this` binding, async behavior, destructuring edge cases, generators, and more.

<span style=" font-size: 1rem;"> \*Discover the answers by clicking on the questions.</span>

## Javascript Coding Interview Questions ES6 ES7 ES8 ES9

### 1. What is the output of this code? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
let p = {};
let q = { id: "q" };
let r = { id: "r" };

p[q] = 10;
p[r] = 20;

console.log(p[q]);
```

<details>
<summary><b>View Answer</b></summary>

The output is `20`.

When a non-string value is used as an object key, JavaScript converts it to a string using `.toString()`. Both `q` and `r` are plain objects, so both convert to `"[object Object]"`.

That means `p[q]` and `p[r]` refer to the same key. The second assignment (`p[r] = 20`) overwrites the first (`p[q] = 10`), so reading `p[q]` returns `20`.

The object `p` internally looks like:

```js
{ "[object Object]": 20 }
```

</details>

### 2. What will this code print? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
let m = { val: "original" };
let n = m;
let o = { ...m };

m.val = "updated";

console.log(n.val);
console.log(o.val);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
updated
original
```

`n = m` creates a reference — both `n` and `m` point to the same object in memory. So mutating `m.val` also changes `n.val`.

`o = { ...m }` uses the spread operator, which performs a **shallow copy**. `o` gets its own copy of the properties at that moment. Future changes to `m` don't affect `o`.

</details>

### 3. Predict the output of this code. <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span>

```js
const config = {
  env: "production",
  getEnv: function () {
    return this.env;
  },
};

const fn = config.getEnv;

console.log(config.getEnv());
console.log(fn());
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
production
undefined
```

When `config.getEnv()` is invoked as a method on the `config` object, `this` inside it refers to `config`, so `this.env` is `"production"`.

When the function reference is stored in `fn` and called as a standalone function (`fn()`), `this` no longer refers to `config`. In non-strict mode, `this` defaults to the global object (`window` in browsers, `global` in Node), which has no `env` property — so the result is `undefined`.

</details>

### 4. What does this code output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const multiply = (x, y) => x * y;

console.log(multiply(3, 4));
console.log(multiply(3));
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
12
NaN
```

The first call passes both arguments, so `3 * 4 = 12`.

The second call passes only one argument (`x = 3`), so `y` is `undefined`. `3 * undefined` evaluates to `NaN` (Not a Number) because arithmetic with `undefined` produces `NaN`.

</details>

### 5. What will the following code print to the console? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const items = ["a", "b", "c"];

for (var i = 0; i < items.length; i++) {
  setTimeout(() => {
    console.log(items[i]);
  }, 500);
}
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
undefined
undefined
undefined
```

Because `var` is function-scoped (not block-scoped), all three `setTimeout` callbacks share the **same** `i` variable. By the time the callbacks execute after 500ms, the loop has finished and `i` is `3`. Since `items[3]` doesn't exist, it's `undefined` — printed three times.

To fix this, use `let` instead of `var`, which creates a new binding per iteration.

</details>

### 6. What does this code output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const nums = [10, 20, 30];

const doubled = nums.map((n) => n * 2);
console.log(doubled);
console.log(nums);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
[20, 40, 60]
[10, 20, 30]
```

`Array.map()` returns a **new array** — it never mutates the original. Each element is multiplied by 2 in `doubled`, while `nums` stays unchanged.

</details>

### 7. Guess the output of this tricky code. <span class="level-tag level-3">Expert <span style='font-size:15px;'>&#128640;</span></span>

```js
let count = 0;

(function loop() {
  if (count++ < 3) {
    setTimeout(loop, 0);
  }
})();

console.log(count);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
1
```

The IIFE runs synchronously. When `count++` is evaluated, `count` is `0` (post-increment means the check uses 0, then increments to 1). The condition `0 < 3` is true, so `setTimeout(loop, 0)` is scheduled. But `setTimeout` is async — it gets pushed to the task queue.

`console.log(count)` runs immediately after the IIFE (still on the call stack), at which point `count` is `1`.

The recursive `loop` calls will continue asynchronously afterwards, but `console.log` has already executed.

> 📖 **Want to go deeper on async execution, closures, and hoisting?** Check out the full [JavaScript Interview Questions 2026](https://stackinterview.dev/guides/javascript-interview-questions-2026) guide on StackInterview.

</details>

### 8. What does this code print? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
let user = { name: "Alice", score: 100 };
let clone = user;

clone.name = "Bob";
clone = { name: "Charlie", score: 200 };

console.log(user.name);
console.log(clone.name);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
Bob
Charlie
```

Initially, `clone = user` — both point to the same object. So `clone.name = "Bob"` mutates the shared object, making `user.name` also `"Bob"`.

Then `clone = { name: "Charlie", score: 200 }` reassigns the `clone` variable to a brand new object. `user` still points to the original (now mutated) object with `name: "Bob"`.

</details>

### 9. What is the output? <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span>

```js
let arr1 = [1, 2, 3];
let arr2 = [1, 2, 3];
let arr3 = arr1;

console.log(arr1 == arr2);
console.log(arr1 === arr2);
console.log(arr1 === arr3);
console.log(arr3 == arr2);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
false
false
true
false
```

In JavaScript, arrays and objects are compared by **reference**, not by value. `arr1` and `arr2` are separate objects in memory, even though they hold identical values — so both `==` and `===` return `false`.

`arr3 = arr1` makes `arr3` point to the **same** object as `arr1`, so `arr1 === arr3` is `true`.

`arr3 == arr2` is still `false` because `arr3` references `arr1`'s object, not `arr2`'s.

</details>

### 10. What will this async code log and in what order? <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span>

```js
var counter = 0;

for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    counter++;
    console.log(counter);
  }, 0);
}
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
1
2
3
```

Unlike `var`, `let` creates a block-scoped binding per iteration — but here it doesn't matter because `counter` is a `var` in the outer scope and is shared across all callbacks.

Each `setTimeout` callback increments `counter` by 1 when it runs. All three callbacks are queued and run sequentially after the synchronous code finishes. So they log `1`, `2`, `3` in order.

</details>

### 11. What does this code print? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const points = [
  { x: 1 },
  { x: 2 },
  { x: 3 },
];

points.forEach((pt) => (pt.x *= 10));

console.log(points[0].x, points[1].x, points[2].x);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
10 20 30
```

`forEach` iterates over the array and the callback receives a reference to each object. Mutating `pt.x` directly modifies the original objects inside the `points` array. So after the loop, all `x` values are multiplied by 10.

</details>

### 12. What is the output of this code? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
let total = 100;

function calculate() {
  var total = 50;
  return total * 2;
}

console.log(calculate());
console.log(total);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
100
100
```

Inside `calculate()`, a **local** `total` is declared with `var` and assigned `50`. The function returns `50 * 2 = 100`. This local variable is completely separate from the global `total = 100`.

After the function call, the outer `total` is still `100`, untouched.

</details>

### 13. What does this code output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const defaults = { theme: "dark", lang: "en" };
const custom = { ...defaults, lang: "fr", fontSize: 14 };

console.log(defaults.lang);
console.log(custom.lang);
console.log(custom.fontSize);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
en
fr
14
```

The spread operator copies all properties from `defaults` into the new `custom` object. Properties listed **after** the spread override the copied values — so `lang: "fr"` replaces `lang: "en"`. `fontSize: 14` is a new property added only to `custom`.

The original `defaults` object is never mutated.

</details>

### 14. What is the output here? <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span>

```js
function greet(name = "stranger", greeting = `Hello, ${name}!`) {
  return greeting;
}

console.log(greet());
console.log(greet("Riya"));
console.log(greet(undefined, "Hey there!"));
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
Hello, stranger!
Hello, Riya!
Hey there!
```

Default parameter values in ES6 can reference **earlier parameters**. When `name` is omitted (or `undefined`), it falls back to `"stranger"`, and `greeting` is computed as `` `Hello, stranger!` ``.

When `"Riya"` is passed, `name = "Riya"` and `greeting = "Hello, Riya!"`.

When `undefined` is explicitly passed for `name`, the default `"stranger"` kicks in, but the second argument `"Hey there!"` overrides the `greeting` default entirely.

</details>

### 15. What does this code output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const role = "admin";
const level = 5;

const profile = { role, level, active: true };
console.log(profile);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```js
{ role: "admin", level: 5, active: true }
```

ES6 **shorthand property syntax** allows you to write `{ role, level }` instead of `{ role: role, level: level }` when the variable name matches the desired key name. Additional explicit properties like `active: true` can be mixed in freely.

</details>

### 16. What is the output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
const colors = ["red", "green", "blue"];
const [primary, , tertiary] = colors;

console.log(primary);
console.log(tertiary);
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
red
blue
```

Array destructuring assigns values positionally. The `,,` (two commas with no variable in between) **skips** the second element (`"green"`). `primary` gets `"red"` (index 0) and `tertiary` gets `"blue"` (index 2).

</details>

### 17. What does this code print? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

```js
console.log(typeof NaN);
console.log(NaN === NaN);
console.log(Number.isNaN(NaN));
console.log(Number.isNaN("hello"));
```

<details>
<summary><b>View Answer</b></summary>

Output:

```
number
false
true
false
```

`typeof NaN` is `"number"` — a well-known JavaScript quirk. `NaN` stands for "Not a Number" but belongs to the number type.

`NaN === NaN` is `false` — `NaN` is the only value in JS that is **not equal to itself**.

`Number.isNaN(NaN)` correctly returns `true`.

`Number.isNaN("hello")` returns `false` — unlike the older global `isNaN()`, `Number.isNaN()` does **not** coerce its argument. It only returns `true` for actual `NaN` values.

> 📖 **Tripped up by `typeof` quirks and NaN behaviour?** The [JavaScript Interview Questions 2026](https://stackinterview.dev/guides/javascript-interview-questions-2026) guide on StackInterview covers all these edge cases in depth.

</details>

## Javascript Programming Questions

<details>
<summary><b>18. Write a function that returns the product of all negative numbers in an array. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

Answer:

```js
function productOfNegatives(numbers) {
  let product = 1;
  let hasNegative = false;

  for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] < 0) {
      product *= numbers[i];
      hasNegative = true;
    }
  }

  return hasNegative ? product : 0;
}

// Example usage:
console.log(productOfNegatives([-2, 3, -4, 5, -1])); // Output: -8
console.log(productOfNegatives([1, 2, 3]));           // Output: 0
```

The function iterates through the array, multiplying together only the negative numbers. A `hasNegative` flag tracks whether any negatives were found — if none were, we return `0` instead of the misleading `1`.

</details>

<details>
<summary><b>19. Write a function that reverses each word in a sentence individually, without reversing the sentence itself. <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span></b></summary>

Answer:

```js
function reverseEachWord(sentence) {
  return sentence
    .split(" ")
    .map((word) => word.split("").reverse().join(""))
    .join(" ");
}

// Example usage:
console.log(reverseEachWord("Hello World from JavaScript"));
// Output: "olleH dlroW morf tpircSavaJ"
```

The sentence is split into words by spaces. Each word is converted to an array of characters, reversed, and joined back. Finally, all words are joined with spaces to reconstruct the sentence.

</details>

<details>
<summary><b>20. Write a function that groups an array of strings by their first letter. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

Answer:

```js
function groupByFirstLetter(arr) {
  return arr.reduce((acc, word) => {
    const key = word[0].toUpperCase();
    if (!acc[key]) acc[key] = [];
    acc[key].push(word);
    return acc;
  }, {});
}

// Example usage:
const words = ["apple", "avocado", "banana", "blueberry", "cherry", "apricot"];
console.log(groupByFirstLetter(words));
// Output:
// { A: ['apple', 'avocado', 'apricot'], B: ['banana', 'blueberry'], C: ['cherry'] }
```

We use `reduce` to build an accumulator object. For each word, we extract its first letter (uppercased) as the key, and push the word into the matching array, creating the array if it doesn't exist yet.

</details>

<details>
<summary><b>21. Write a function that checks if two strings are anagrams of each other. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

Answer:

```js
function areAnagrams(str1, str2) {
  const normalize = (s) => s.toLowerCase().split("").sort().join("");
  return normalize(str1) === normalize(str2);
}

// Example usage:
console.log(areAnagrams("listen", "silent")); // Output: true
console.log(areAnagrams("hello", "world"));   // Output: false
```

An anagram uses the same characters in a different order. By lowercasing, splitting into characters, sorting alphabetically, and joining back — we get a "normalized signature" for each string. If both signatures match, the strings are anagrams.

</details>

<details>
<summary><b>22. Write a function that returns the most frequently occurring element in an array. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

Answer:

```js
function mostFrequent(arr) {
  const freq = {};

  for (const item of arr) {
    freq[item] = (freq[item] || 0) + 1;
  }

  return Object.keys(freq).reduce((a, b) => (freq[a] > freq[b] ? a : b));
}

// Example usage:
const votes = ["Alice", "Bob", "Alice", "Charlie", "Bob", "Alice"];
console.log(mostFrequent(votes)); // Output: "Alice"
```

We first build a frequency map, counting each element's occurrences. Then we use `reduce` over the keys to find the one with the highest count.

</details>

<details>
<summary><b>23. Write a function that deeply flattens a nested array. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

Answer:

```js
function deepFlatten(arr) {
  return arr.reduce((flat, item) => {
    return flat.concat(Array.isArray(item) ? deepFlatten(item) : item);
  }, []);
}

// Example usage:
console.log(deepFlatten([1, [2, [3, [4]], 5], 6]));
// Output: [1, 2, 3, 4, 5, 6]

// ES2019 native alternative:
// [1, [2, [3]]].flat(Infinity)
```

The function recursively processes each element. If an element is itself an array, it calls `deepFlatten` on it again before concatenating. The recursion bottoms out when all elements are primitives.

ES2019 introduced `Array.flat(Infinity)` as a built-in way to achieve the same result.

</details>

<details>
<summary><b>24. Write a function that implements a basic `pipe` utility — composing functions left to right. <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span></b></summary>

Answer:

```js
function pipe(...fns) {
  if (fns.length === 0) return (value) => value;
  return (value) => fns.reduce((acc, fn) => fn(acc), value);
}

// Example usage:
const double = (x) => x * 2;
const addTen = (x) => x + 10;
const square = (x) => x * x;

const transform = pipe(double, addTen, square);
console.log(transform(3)); // (3*2 + 10)² = 16² = 256

// Edge case: pipe with no functions acts as identity
const identity = pipe();
console.log(identity(42)); // 42
```

`pipe` takes any number of functions and returns a new function. When invoked with a value, it passes that value through each function from left to right, feeding each result as the input to the next — similar to how Unix pipes work. The empty-array guard returns an identity function rather than throwing on `fns.reduce` with no initial value.

</details>

<details>
<summary><b>25. Write a function that validates whether a string contains balanced brackets. <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span></b></summary>

Answer:

```js
function isBalanced(str) {
  const map = { ")": "(", "}": "{", "]": "[" };
  const stack = [];

  for (const char of str) {
    if ("({[".includes(char)) {
      stack.push(char);
    } else if (")}]".includes(char)) {
      if (stack.pop() !== map[char]) return false;
    }
  }

  return stack.length === 0;
}

// Example usage:
console.log(isBalanced("({[hello]})")); // Output: true
console.log(isBalanced("({[})"));       // Output: false
console.log(isBalanced(""));            // Output: true
```

We use a stack. Opening brackets are pushed in. When a closing bracket is encountered, we pop the last item from the stack and check if it matches the expected opening pair. If anything mismatches, or the stack isn't empty at the end, the brackets are not balanced.

</details>

<details>
<summary><b>26. Write a function that converts an object into a query string. <span class="level-tag level-3">Expert <span style='font-size:15px;'>&#128640;</span></span></b></summary>

Answer:

```js
function toQueryString(params) {
  return Object.entries(params)
    .filter(([, value]) => value !== undefined && value !== null)
    .map(([key, value]) => `${encodeURIComponent(key)}=${encodeURIComponent(value)}`)
    .join("&");
}

// Example usage:
const filters = { search: "hello world", page: 2, sort: "asc" };
console.log(toQueryString(filters));
// Output: "search=hello%20world&page=2&sort=asc"

// Handles null/undefined gracefully:
console.log(toQueryString({ q: "js", debug: undefined }));
// Output: "q=js"
```

`Object.entries()` returns `[key, value]` pairs. A `filter` step drops `null` and `undefined` values that would otherwise produce `key=null` in the string. Each remaining pair is URL-encoded with `encodeURIComponent`.

> ⚠️ **Limitation:** this implementation handles flat objects with primitive values only. Array values (e.g. `{ tags: ["a","b"] }`) and nested objects require a recursive approach or a library like `qs`.

</details>

<details>
<summary><b>27. Write a function that debounces another function. <span class="level-tag level-3">Expert <span style='font-size:15px;'>&#128640;</span></span></b></summary>

Answer:

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// Example usage:
const log = debounce((msg) => console.log(msg), 300);

log("first");
log("second");
log("third"); // Only "third" will be logged after 300ms
```

Debouncing ensures a function is only executed **after** a specified delay following the last call. Each new call resets the timer using `clearTimeout`. This is extremely common in search inputs, resize handlers, and scroll events to reduce unnecessary executions.

`fn.apply(this, args)` preserves the calling context and any arguments passed in.

> 🚀 **Debounce, throttle, memoization — these come up constantly in senior roles.** See how they fit into the bigger picture in the [Senior Frontend Interview Topics 2026](https://stackinterview.dev/guides/senior-frontend-interview-topics-2026) guide on StackInterview.

</details>

<details>
<summary><b>28. What is the output of this code? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
const x = 5;
{
  const x = 15;
  console.log(x);
}
console.log(x);
```

Output:

```
15
5
```

`const` (like `let`) is **block-scoped**. The inner `const x = 15` exists only within the curly-brace block. Outside that block, the outer `const x = 5` is still in scope. The two `x` variables are completely independent.

</details>

<details>
<summary><b>29. Predict the output. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
const settings = { volume: 80, brightness: 60, wifi: true };
const { volume, wifi } = settings;
console.log(volume, wifi);
```

Output:

```
80 true
```

Object destructuring extracts named properties from the object and assigns them to local variables with matching names. `volume` gets `80`, `wifi` gets `true`, and `brightness` is simply not destructured — it stays in `settings`.

</details>

<details>
<summary><b>30. What is the output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
const first = [1, 2, 3];
const second = [4, 5, 6];
const merged = [...first, 0, ...second];
console.log(merged);
```

Output:

```
[1, 2, 3, 0, 4, 5, 6]
```

The spread operator unpacks the elements of `first` and `second` into the new `merged` array. The literal `0` is inserted between the two spreads, landing at index 3. Original arrays are not affected.

</details>

<details>
<summary><b>31. What does this output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
const original = [10, 20, 30];
const copy = [...original];

copy[0] = 99;

console.log(original[0]);
console.log(copy[0]);
```

Output:

```
10
99
```

`[...original]` creates a **shallow copy** — a new array with the same primitive values. Changing `copy[0]` does not affect `original[0]` because primitive values (numbers) are copied by value, not by reference.

(Note: if the elements were objects, this would behave differently — nested objects are still shared references in a shallow copy.)

</details>

<details>
<summary><b>32. What is the output of the following code? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
function run() {
  console.log(value);
  let value = 42;
}

run();
```

Output:

```
ReferenceError: Cannot access 'value' before initialization
```

Variables declared with `let` and `const` are subject to the **Temporal Dead Zone (TDZ)**. Even though the declaration is hoisted, the variable is not accessible until the line where it is initialized. Accessing it before that line throws a `ReferenceError`.

This is different from `var`, which would have logged `undefined` due to initialization hoisting.

> 📖 **The TDZ catches a lot of developers off guard.** Brush up on `var` vs `let` vs `const` hoisting rules in the [JavaScript Interview Questions 2026](https://stackinterview.dev/guides/javascript-interview-questions-2026) guide on StackInterview.

</details>

<details>
<summary><b>33. Predict the output. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
const base = [1, 2, 3];
const ref = base;

ref.push(4);

console.log(base);
console.log(ref);
```

Output:

```
[1, 2, 3, 4]
[1, 2, 3, 4]
```

`ref = base` does **not** copy the array — it copies the **reference**. Both `base` and `ref` point to the exact same array in memory. Calling `ref.push(4)` mutates that shared array, so both variables reflect the change.

To avoid this, use `const ref = [...base]` or `base.slice()` to create an independent copy.

</details>

<details>
<summary><b>34. What does this code output? <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span></b></summary>

```js
const employees = [
  { id: 1, dept: "Engineering" },
  { id: 2, dept: "Design" },
  { id: 3, dept: "Engineering" },
];

employees.sort((a, b) => (a.dept < b.dept ? -1 : 1));
console.log(employees.map((e) => e.dept));
```

Output:

```
["Design", "Engineering", "Engineering"]
```

The comparator returns `-1` when `a.dept` comes before `b.dept` alphabetically (ascending order). `"Design"` < `"Engineering"` lexicographically, so it moves to the front.

`sort` mutates the original array in place, and then `map` extracts just the `dept` strings.

</details>

<details>
<summary><b>35. What is the output of these `typeof` checks? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
console.log(typeof 0);
console.log(typeof "");
console.log(typeof false);
console.log(typeof Symbol());
console.log(typeof undefined);
console.log(typeof null);
console.log(typeof {});
console.log(typeof []);
console.log(typeof function () {});
```

Output:

```
number
string
boolean
symbol
undefined
object
object
object
function
```

Key quirks to note:

- `typeof null` is `"object"` — a historical bug in JavaScript that can't be corrected for backward compatibility.
- `typeof []` is `"object"` — arrays are objects. Use `Array.isArray([])` to specifically check for arrays.
- `typeof Symbol()` is `"symbol"` — Symbols introduced in ES6 have their own type.
- `typeof function() {}` is `"function"` — even though functions are objects internally, `typeof` has a special case for them.

</details>

<details>
<summary><b>36. Write a function that accurately identifies the true type of any value, returning "array" for arrays and "null" for null. <span class="level-tag level-3">Expert <span style='font-size:15px;'>&#128640;</span></span></b></summary>

```js
console.log(trueType(42));
console.log(trueType("hi"));
console.log(trueType([]));
console.log(trueType({}));
console.log(trueType(null));
console.log(trueType(undefined));
console.log(trueType(() => {}));
```

Answer:

```js
const trueType = (val) => {
  if (val === null) return "null";
  if (val === undefined) return "undefined";
  if (Array.isArray(val)) return "array";
  return typeof val;
};
```

Output:

```
number
string
array
object
null
undefined
function
```

`typeof` alone is insufficient because it reports both arrays and `null` as `"object"`. This utility handles each edge case explicitly:

- `null` is checked first with strict equality — `typeof null === "object"` is a historical JS bug.
- `undefined` is checked before any property access — `undefined?.constructor` would return `undefined`, not throw, but `undefined.constructor.name` without optional chaining **does** throw a `TypeError`. The original one-liner using `val?.constructor.name` silently returns `undefined` for both `null` and `undefined` without the explicit guards — making the guards load-bearing, not optional.
- `Array.isArray()` correctly identifies arrays before the `typeof` fallback.
- Everything else falls through to `typeof`, which correctly handles `"number"`, `"string"`, `"boolean"`, `"function"`, `"symbol"`, and `"bigint"`.

</details>

<details>
<summary><b>37. What will this code output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
function init() {
  console.log(result);
  var a = 5;
  let b = 10;
  const c = 15;
  var result = a + b + c;
}

init();
```

- A: 30
- B: undefined
- C: ReferenceError
- D: TypeError

Answer — **B: undefined**

`var` declarations are **hoisted** to the top of the function with an initial value of `undefined`. So when `console.log(result)` executes, `result` exists in scope but hasn't been assigned yet — it's `undefined`.

Effectively, JavaScript interprets the code like:

```js
function init() {
  var result; // hoisted
  console.log(result); // undefined

  var a = 5;
  let b = 10;
  const c = 15;
  result = a + b + c; // assigned later
}
```

`let` and `const` are also hoisted, but they remain in the **Temporal Dead Zone** until their declaration line — accessing them before would throw a `ReferenceError`.

> 📖 **Hoisting is one of the most tested JavaScript concepts in interviews.** Get the full breakdown in the [JavaScript Interview Questions 2026](https://stackinterview.dev/guides/javascript-interview-questions-2026) guide on StackInterview.

</details>

<details>
<summary><b>38. What will be the output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
let score = 50;

function adjust() {
  if (true) {
    let score = 100;
    console.log(score);
  }
  console.log(score);
}

adjust();
```

- A: 100, 50
- B: 100, 100
- C: 50, 50
- D: 50, 100

Answer — **A: 100, 50**

`let` is block-scoped. The `score = 100` inside the `if` block is a **new, separate variable** that shadows the outer `score = 50` only within that block.

- Inside the `if` block: logs `100` (inner `score`).
- Outside the block: logs `50` (outer `score`, unchanged).

</details>

<details>
<summary><b>39. What is the output? <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span></b></summary>

```js
const api = {
  baseUrl: "https://example.com",
  getUrl: function () {
    return this.baseUrl;
  },
};

Object.freeze(api);
api.baseUrl = "https://hacked.com";
api.version = "v2";

console.log(api.baseUrl);
console.log(api.version);
```

- A: "https://hacked.com", "v2"
- B: "https://example.com", undefined
- C: TypeError
- D: ReferenceError

Answer — **B: "https://example.com", undefined**

`Object.freeze()` prevents any additions, deletions, or modifications to the object's properties.

- `api.baseUrl = "https://hacked.com"` — silently ignored in non-strict mode; throws `TypeError` in strict mode.
- `api.version = "v2"` — also silently ignored; `version` is never added.

So `api.baseUrl` remains `"https://example.com"`, and `api.version` is `undefined`.

</details>

<details>
<summary><b>40. What is the output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
function outer() {
  const prefix = "Hello";

  function inner(name) {
    return `${prefix}, ${name}!`;
  }

  return inner("World");
}

console.log(outer());
```

- A: "Hello, World!"
- B: undefined, World!
- C: ReferenceError
- D: "prefix, World!"

Answer — **A: "Hello, World!"**

`inner` is defined inside `outer` and has access to `outer`'s scope through **closure**. Even though `prefix` is declared in the outer function, `inner` can read it via the scope chain.

`outer()` invokes `inner("World")`, which returns `"Hello, World!"`. That string is then returned by `outer()` and logged.

</details>

<details>
<summary><b>41. What is the output? <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
var flag = true;

function check() {
  console.log(flag);

  if (false) {
    var flag = false;
  }
}

check();
```

- A: true
- B: false
- C: undefined
- D: ReferenceError

Answer — **C: undefined**

This is a classic `var` hoisting trap. Even though the `if (false)` block never executes, the `var flag` declaration inside it is **still hoisted** to the top of `check()`. This creates a **local** `flag` variable that shadows the global one.

At the time `console.log(flag)` runs, the local `flag` exists but hasn't been assigned — so it's `undefined`.

> 📖 **`var` hoisting inside dead code blocks is a favourite interview trick.** Master it alongside other scope quirks in the [JavaScript Interview Questions 2026](https://stackinterview.dev/guides/javascript-interview-questions-2026) guide on StackInterview.

</details>

<details>
<summary><b>42. What does this output? <span class="level-tag level-2">Intermediate <span style='font-size:15px;'>&#128641;</span></span></b></summary>

```js
const data = {
  user: "John",
  address: {
    city: "Mumbai",
    pin: 400001,
  },
};

const {
  user,
  address: { city, pin: postalCode },
} = data;

console.log(user, city, postalCode);
```

- A: John Mumbai 400001
- B: John Mumbai postalCode
- C: undefined undefined undefined
- D: SyntaxError

Answer — **A: John Mumbai 400001**

This is **nested destructuring with renaming**:
- `user` extracts `data.user` → `"John"`.
- `address: { city }` dives into `data.address` and extracts `city` → `"Mumbai"`.
- `pin: postalCode` extracts `data.address.pin` and assigns it to a variable named `postalCode` → `400001`.

The key `address` itself is not bound to any variable — only its nested properties are.

</details>

<details>
<summary><b>43. What is the output of this generator code? <span class="level-tag level-3">Expert <span style='font-size:15px;'>&#128640;</span></span></b></summary>

```js
function* counter() {
  let n = 0;
  while (true) {
    yield n++;
  }
}

const gen = counter();
console.log(gen.next().value);
console.log(gen.next().value);
console.log(gen.next().value);
```

- A: 0, 1, 2
- B: 1, 2, 3
- C: 0, 0, 0
- D: Infinite loop — crashes the browser

Answer — **A: 0, 1, 2**

Even though the generator has a `while (true)` loop, it does **not** run forever on each call. Generators are **lazy** — they pause at every `yield` and resume only when `.next()` is called.

- First `.next()`: enters loop, yields `0` (n becomes 1), pauses.
- Second `.next()`: resumes, yields `1` (n becomes 2), pauses.
- Third `.next()`: resumes, yields `2` (n becomes 3), pauses.

This pattern is useful for creating infinite sequences without blocking execution.

</details>

<details>
<summary><b>44. Write a function that supports both curried and multi-argument calling for summing numbers. <span class="level-tag level-3">Expert <span style='font-size:15px;'>&#128640;</span></span></b></summary>

```js
console.log(add(1, 2, 3).value);      // 6
console.log(add(1)(2)(3).value);      // 6
console.log(add(1, 2)(3).value);      // 6
```

Answer:

```js
function add(...args) {
  const total = args.reduce((sum, n) => sum + n, 0);

  function next(...moreArgs) {
    return add(total, ...moreArgs);
  }

  next.value = total;
  return next;
}

console.log(add(1, 2, 3).value);   // 6
console.log(add(1)(2)(3).value);   // 6
console.log(add(1, 2)(3).value);   // 6
```

The `add` function:
1. Sums all arguments passed using `reduce`.
2. Returns a `next` function that, when called again, recursively calls `add` with the accumulated total plus the new arguments.
3. Exposes the current total via `.value` on the returned function.

This supports any combination of batched or curried calls.

> 🚀 **Currying and function composition are staple senior frontend interview topics.** See more patterns like this in the [Senior Frontend Interview Topics 2026](https://stackinterview.dev/guides/senior-frontend-interview-topics-2026) guide on StackInterview.

</details>

<details>
<summary><b>45. Understanding the `.reduce()` Method Variations <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
const transactions = [200, -50, 300, -100, 150];

// Task 1: Get the final balance (sum of all transactions)
// Task 2: Get the total of only deposits (positive values)
// Task 3: Find the largest single transaction
// Task 4: Count how many transactions are withdrawals (negative)
```

Answer:

Task 1: Final balance

```js
const balance = transactions.reduce((acc, val) => acc + val, 0);
console.log(balance); // 500
```

Task 2: Total deposits only

```js
const totalDeposits = transactions.reduce((acc, val) => (val > 0 ? acc + val : acc), 0);
console.log(totalDeposits); // 650
```

Task 3: Largest transaction

```js
const largest = transactions.reduce((max, val) => (val > max ? val : max), -Infinity);
console.log(largest); // 300
```

Task 4: Count withdrawals

```js
const withdrawalCount = transactions.reduce((count, val) => (val < 0 ? count + 1 : count), 0);
console.log(withdrawalCount); // 2
```

These variations show how `reduce` can serve as a Swiss-army knife for array operations — replacing `filter + length`, `Math.max`, and `forEach` counters with a single traversal.

> 🚀 **Knowing when to reach for `reduce` over `filter/map/forEach` is a sign of a senior engineer.** Explore more advanced patterns in the [Senior Frontend Interview Topics 2026](https://stackinterview.dev/guides/senior-frontend-interview-topics-2026) guide on StackInterview.

</details>

<details>
<summary><b>46. Identify and fix the bug in this code. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span></b></summary>

```js
let prices = [100, 200, 300, 400, 500];
prices = prices.map((price) => price * 1.1);
console.log(prices.reduce((sum, price) => sum + price));
```

Answer:

```js
// Output: 1650.0000000000002
```

Explanation:

1. `prices` starts as `[100, 200, 300, 400, 500]`.
2. `.map(price => price * 1.1)` applies a 10% increase to each. Due to **floating point representation**, `100 * 1.1` is not exactly `110` in JavaScript — it's `110.00000000000001`. So the mapped array is `[110.00000000000001, 220.00000000000003, 330.00000000000006, 440.00000000000006, 550]` (values vary by engine).
3. `.reduce((sum, price) => sum + price)` sums all values, accumulating the floating point error.
4. The actual output is `1650.0000000000002`, not `1650`.

**Two bugs to flag here:**
- **Floating point**: multiplying by `1.1` introduces IEEE 754 precision errors. Use `Math.round(price * 1.1 * 100) / 100` or a decimal library for financial calculations.
- **Missing initial value**: omitting the second argument to `reduce` uses the first element as the starting accumulator, which throws on an empty array. Always pass `0`: `.reduce((sum, price) => sum + price, 0)`.

```js
// Corrected version:
let prices = [100, 200, 300, 400, 500];
prices = prices.map((price) => Math.round(price * 1.1 * 100) / 100);
console.log(prices.reduce((sum, price) => sum + price, 0)); // 1650
```

</details>

<details>
<summary><b>47. Write a JavaScript function that takes an array and returns only the unique values — without using `Set` or `filter`. <span class="level-tag level-1">Beginner <span style='font-size:15px;'>&#128642;</span></span>

_\*Cannot use `Set`, `filter`, or `includes`._</b></summary>

Answer:

Approach 1: Using an object as a frequency map

```js
function getUniques(arr) {
  const seen = Object.create(null); // avoids prototype key collisions
  const result = [];

  for (let i = 0; i < arr.length; i++) {
    const key = String(arr[i]);
    if (!Object.prototype.hasOwnProperty.call(seen, key)) {
      seen[key] = true;
      result.push(arr[i]);
    }
  }

  return result;
}

console.log(getUniques([1, 2, 2, 3, 1, 4]));       // [1, 2, 3, 4]
console.log(getUniques([0, false, "", 0, false]));  // [0, false, ""]
```

> ⚠️ **Why the original `!seen[arr[i]]` is buggy:** falsy values like `0`, `""`, and `false` evaluate to falsy even after being added to `seen`, so they'd be incorrectly treated as "not seen" on subsequent iterations and pushed again. Using `hasOwnProperty` checks for key existence regardless of the stored value.

Approach 2: Using `reduce` with an object tracker

```js
function getUniquesReduce(arr) {
  return arr.reduce((acc, val) => {
    const key = String(val);
    if (!Object.prototype.hasOwnProperty.call(acc.seen, key)) {
      acc.seen[key] = true;
      acc.result.push(val);
    }
    return acc;
  }, { seen: Object.create(null), result: [] }).result;
}

console.log(getUniquesReduce([5, 5, 6, 7, 6]));        // [5, 6, 7]
console.log(getUniquesReduce([0, "", false, 0, ""]));   // [0, "", false]
```

Both approaches use a null-prototype object (`Object.create(null)`) as a lookup map — this avoids accidental collisions with inherited prototype keys like `"constructor"` or `"toString"`. Key existence is checked with `hasOwnProperty` rather than truthiness to correctly handle falsy values.

</details>
