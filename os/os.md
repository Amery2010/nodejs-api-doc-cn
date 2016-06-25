# 方法和属性

* [os.EOL](#EOL)
* [os.type()](#type)
* [os.release()](#release)
* [os.platform()](#platform)
* [os.cpus()](#cpus)
* [os.arch()](#arch)
* [os.endianness()](#endianness)
* [os.loadavg()](#loadavg)
* [os.totalmem()](#totalmem)
* [os.freemem()](#freemem)
* [os.uptime()](#uptime)
* [os.networkInterfaces()](#networkInterfaces)
* [os.hostname()](#hostname)
* [os.homedir()](#homedir)
* [os.tmpdir()](#tmpdir)

--------------------------------------------------


<div id="EOL" class="anchor"></div>
## os.EOL

一个定义了操作系统相应的行尾（End-of-line）标识的常量。


<div id="type" class="anchor"></div>
## os.type()

返回操作系统名称。比如 Linux 中返回 `'Linux'`，OS X 中返回 `'Darwin'`，Windows 中返回 `'Windows_NT'`。


<div id="release" class="anchor"></div>
## os.release()

返回操作系统版本。


<div id="platform" class="anchor"></div>
## os.platform()

返回操作系统平台。可能的值有 `'darwin'`，`'freebsd'`，`'linux'`，`'sunos'` 和 `'win32'`。返回 [process.platform](../process/process.md#platform) 的值。


<div id="cpus" class="anchor"></div>
## os.cpus()

返回一个对象数组，包含所安装的每个 CPU/内核的信息：型号、速度（单位 MHz）、时间（一个包含 user、nice、sys、idle 和 irq 所使用 CPU/内核毫秒数的对象）。

os.cpus 的示例：

```
[{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 252020,
        nice: 0,
        sys: 30340,
        idle: 1070356870,
        irq: 0
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 306960,
        nice: 0,
        sys: 26980,
        idle: 1071569080,
        irq: 0
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 248450,
        nice: 0,
        sys: 21750,
        idle: 1070919370,
        irq: 0
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 256880,
        nice: 0,
        sys: 19430,
        idle: 1070905480,
        irq: 20
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 511580,
        nice: 20,
        sys: 40900,
        idle: 1070842510,
        irq: 0
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 291660,
        nice: 0,
        sys: 34360,
        idle: 1070888000,
        irq: 10
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 308260,
        nice: 0,
        sys: 55410,
        idle: 1071129970,
        irq: 880
    }
},
{
    model: 'Intel(R) Core(TM) i7 CPU         860  @ 2.80GHz',
    speed: 2926,
    times: {
        user: 266450,
        nice: 1480,
        sys: 34920,
        idle: 1072572010,
        irq: 30
    }
}]
```


<div id="arch" class="anchor"></div>
## os.arch()

返回操作系统的 CPU 架构。可能的值有 `'x64'`，`'arm'` 和 `'ia32'`。返回 [process.arch](../process/process.md#arch) 的值。


<div id="endianness" class="anchor"></div>
## os.endianness()

返回 CPU 的字节序。可能值有大端的 `BE` 和小端的 `LE`。


<div id="loadavg" class="anchor"></div>
## os.loadavg()

返回一个包含 1、5、15 分钟平均负载的数组。

平均负载是一个系统活跃度指标，它由操作系统计算并表示为一个小数。根据经验，平均负载最好应小于系统逻辑 CPU 的数量。

平均负载是一个非常 UNIX-y 的概念；在 Windows 平台上没有真正的等价物。这就是为什么这个函数在 Windows 上始终返回 `[0，0，0]` 的原因。


<div id="totalmem" class="anchor"></div>
## os.totalmem()

返回系统内存总量，以字节为单位。


<div id="totalmem" class="anchor"></div>
## os.freemem()

返回可用的系统内存量，以字节为单位。


<div id="uptime" class="anchor"></div>
## os.uptime()

返回操作系统的运行时间，以秒为单位。


<div id="networkInterfaces" class="anchor"></div>
## os.networkInterfaces()

获取网络接口列表：

```
{
    lo: [{
            address: '127.0.0.1',
            netmask: '255.0.0.0',
            family: 'IPv4',
            mac: '00:00:00:00:00:00',
            internal: true
        },
        {
            address: '::1',
            netmask: 'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff',
            family: 'IPv6',
            mac: '00:00:00:00:00:00',
            internal: true
        }],
    eth0: [{
            address: '192.168.1.108',
            netmask: '255.255.255.0',
            family: 'IPv4',
            mac: '01:02:03:0a:0b:0c',
            internal: false
        },
        {
            address: 'fe80::a00:27ff:fe4e:66a1',
            netmask: 'ffff:ffff:ffff:ffff::',
            family: 'IPv6',
            mac: '01:02:03:0a:0b:0c',
            internal: false
        }]
}
```

需要注意的是，由于底层的实现，这里只会返回已分配地址的网络接口。


<div id="hostname" class="anchor"></div>
## os.hostname()

返回操作系统的主机名。


<div id="homedir" class="anchor"></div>
## os.homedir()

返回当前用户的主目录。


<div id="tmpdir" class="anchor"></div>
## os.tmpdir()

返回操作系统默认的临时文件目录。