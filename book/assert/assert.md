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

深度比较 `actual` 和 `expected` 参数，使用比较运算符（`==`）比较原始值。

只考虑可枚举的“自身”属性。`deepEqual()` 的实现不测试对象的原型，连接符号，或不可枚举的属性。这会导致一些潜在的出人意料的结果。例如，下面的例子不会抛出 `AssertionError`，因为 [Error](../errors/class_Error.md#) 对象的属性是不可枚举：

```javascript
// WARNING: This does not throw an AssertionError!
assert.deepEqual(Error('a'), Error('b'));
```

深度比较意味着子对象的可枚举“自身”的属性也会进行评估：

```javascript
const assert = require('assert');

const obj1 = {
    a: {
        b: 1
    }
};
const obj2 = {
    a: {
        b: 2
    }
};
const obj3 = {
    a: {
        b: 1
    }
}
const obj4 = Object.create(obj1);

assert.deepEqual(obj1, obj1);
// OK, object is equal to itself

assert.deepEqual(obj1, obj2);
// AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }
// values of b are different

assert.deepEqual(obj1, obj3);
// OK, objects are equal

assert.deepEqual(obj1, obj4);
// AssertionError: { a: { b: 1 } } deepEqual {}
// Prototypes are ignored
```

如果这两个值不相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="deepStrictEqual" class="anchor"></div>
## assert.deepStrictEqual(actual, expected[, message])

一般情况下等同于 `assert.deepEqual()`，但有两个例外。首先，原始值是使用全等运算符（`===`）进行比较。其次，比较的对象包括严格比较他们的原型。

```javascript
const assert = require('assert');

assert.deepEqual({a:1}, {a:'1'});
  // OK, because 1 == '1'

assert.deepStrictEqual({a:1}, {a:'1'});
  // AssertionError: { a: 1 } deepStrictEqual { a: '1' }
  // because 1 !== '1' using strict equality
```

如果这两个值不相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="notDeepEqual" class="anchor"></div>
## assert.notDeepEqual(actual, expected[, message])

深度地不相等比较测试，与 [assert.deepEqual()](#deepEqual) 相反。

```javascript
const assert = require('assert');

const obj1 = {
    a: {
        b: 1
    }
};
const obj2 = {
    a: {
        b: 2
    }
};
const obj3 = {
    a: {
        b: 1
    }
}
const obj4 = Object.create(obj1);

assert.notDeepEqual(obj1, obj1);
// AssertionError: { a: { b: 1 } } notDeepEqual { a: { b: 1 } }

assert.notDeepEqual(obj1, obj2);
// OK, obj1 and obj2 are not deeply equal

assert.notDeepEqual(obj1, obj3);
// AssertionError: { a: { b: 1 } } notDeepEqual { a: { b: 1 } }

assert.notDeepEqual(obj1, obj4);
// OK, obj1 and obj2 are not deeply equal
```

如果这两个值深度相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。


<div id="notDeepStrictEqual" class="anchor"></div>
## assert.notDeepStrictEqual(actual, expected[, message])

深度地严格不相等比较测试，与 [assert.deepStrictEqual()](#deepStrictEqual) 相反。

```javascript
const assert = require('assert');

assert.notDeepEqual({a:1}, {a:'1'});
  // AssertionError: { a: 1 } notDeepEqual { a: '1' }

assert.notDeepStrictEqual({a:1}, {a:'1'});
  // OK
```

如果这两个值深度严格相等，将会抛出一个带有 `message` 属性（等于该 `message` 参数的值）的 `AssertionError`。如果 `message` 参数为 `undefined`，将会分配一个默认的错误消息。