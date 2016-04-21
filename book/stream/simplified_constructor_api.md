# 简化的构造函数API

* [Readable](#Readable)
* [Writable](#Writable)
* [Duplex](#Duplex)
* [Transform](#Transform)

--------------------------------------------------


在简单的情况下，不通过继承来构建流，现在会有一些额外的好处。

这可以通过传递适当的方法作为构造选项来实现：

例子：


<div id="Readable" class="anchor"></div>
## Readable

```javascript
var readable = new stream.Readable({
    read: function (n) {
        // sets this._read under the hood

        // push data onto the read queue, passing null
        // will signal the end of the stream (EOF)
        this.push(chunk);
    }
});
```


<div id="Writable" class="anchor"></div>
## Writable

```javascript
var writable = new stream.Writable({
    write: function (chunk, encoding, next) {
        // sets this._write under the hood

        // An optional error can be passed as the first argument
        next()
    }
});

// or

var writable = new stream.Writable({
    writev: function (chunks, next) {
        // sets this._writev under the hood

        // An optional error can be passed as the first argument
        next()
    }
});
```


<div id="Duplex" class="anchor"></div>
## Duplex

```javascript
var duplex = new stream.Duplex({
    read: function (n) {
        // sets this._read under the hood

        // push data onto the read queue, passing null
        // will signal the end of the stream (EOF)
        this.push(chunk);
    },
    write: function (chunk, encoding, next) {
        // sets this._write under the hood

        // An optional error can be passed as the first argument
        next()
    }
});

// or

var duplex = new stream.Duplex({
    read: function (n) {
        // sets this._read under the hood

        // push data onto the read queue, passing null
        // will signal the end of the stream (EOF)
        this.push(chunk);
    },
    writev: function (chunks, next) {
        // sets this._writev under the hood

        // An optional error can be passed as the first argument
        next()
    }
});
```


<div id="Transform" class="anchor"></div>
## Transform

```javascript
var transform = new stream.Transform({
    transform: function (chunk, encoding, next) {
        // sets this._transform under the hood

        // generate output as many times as needed
        // this.push(chunk);

        // call when the current chunk is consumed
        next();
    },
    flush: function (done) {
        // sets this._flush under the hood

        // generate output as many times as needed
        // this.push(chunk);

        done();
    }
});
```