# 方法和属性

* [assert(value[, message])](#assert)
* [assert.equal(actual, expected[, message])](#equal)
* [assert.strictEqual(actual, expected[, message])](#strictEqual)
* [assert.notEqual(actual, expected[, message])](#notEqual)
* [assert.notStrictEqual(actual, expected[, message])](#notStrictEqual)
* [assert.deepEqual(actual, expected[, message])](#deepEqual)
* [assert.deepStrictEqual(actual, expected[, message])](#deepStrictEqual)
* [assert.notDeepEqual(actual, expected[, message])](#notDeepEqual)
* [assert.notDeepStrictEqual(actual, expected[, message])](#notDeepStrictEqual)
* [assert.ok(value[, message])](#ok)
* [assert.fail(actual, expected, message, operator)](#fail)
* [assert.ifError(value)](#ifError)
* [assert.throws(block[, error][, message])](#throws)
* [assert.doesNotThrow(block[, error][, message])](#doesNotThrow)

--------------------------------------------------


<div id="assert" class="anchor"></div>
## assert(value[, message])

[assert.ok()](#ok) 的别名。

```javascript
const assert = require('assert');

assert(true);  // OK
assert(1);     // OK
assert(false);
  // throws "AssertionError: false == true"
assert(0);
  // throws "AssertionError: 0 == true"
assert(false, 'it\'s false');
  // throws "AssertionError: it's false"
```


<div id="equal" class="anchor"></div>
## assert.equal(actual, expected[, message])

浅测试，使用等于比较运算符（`==`）来比较 `actual` 和 `expected` 参数。

```javascript
const assert = require('assert');

assert.equal(1, 1);
  // OK, 1 == 1
assert.equal(1, '1');
  // OK, 1 == '1'

assert.equal(1, 2);
  // AssertionError: 1 == 2
assert.equal({a: {b: 1}}, {a: {b: 1}});
  //AssertionError: { a: { b: 1 } } == { a: { b: 1 } }
```

如果这两个值不相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="strictEqual" class="anchor"></div>
## assert.strictEqual(actual, expected[, message])

严格相等测试，由全等运算符确定（`===`）。

```javascript
const assert = require('assert');

assert.strictEqual(1, 2);
  // AssertionError: 1 === 2

assert.strictEqual(1, 1);
  // OK

assert.strictEqual(1, '1');
  // AssertionError: 1 === '1'
```

如果这两个值不是严格相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="notEqual" class="anchor"></div>
## assert.notEqual(actual, expected[, message])

浅测试，使用不等于比较运算符（`!=`）比较。

```javascript
const assert = require('assert');

assert.notEqual(1, 2);
  // OK

assert.notEqual(1, 1);
  // AssertionError: 1 != 1

assert.notEqual(1, '1');
  // AssertionError: 1 != '1'
```

如果这两个值相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="notStrictEqual" class="anchor"></div>
## assert.notStrictEqual(actual, expected[, message])

严格不相等测试，由不全等运算符确定（`===`）。

```javascript
const assert = require('assert');

assert.notStrictEqual(1, 2);
  // OK

assert.notStrictEqual(1, 1);
  // AssertionError: 1 != 1

assert.notStrictEqual(1, '1');
  // OK
```

如果这两个值严格相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="deepEqual" class="anchor"></div>
## assert.deepEqual(actual, expected[, message])

