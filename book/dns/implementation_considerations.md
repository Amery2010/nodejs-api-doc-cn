# 实现中的注意事项

尽管 [dns.lookup()](./dns.md#lookup) 和每个 `dns.resolve*()/dns.reverse()` 函数都具有网络名称与网络地址相关联的同一个目标（或反之亦然），但是他们的行为却是完全不同的。这些差异可能对 Node.js 程序的行为产生微妙而显著的后果。


<div id="lookup" class="anchor"></div>
### dns.lookup()

[dns.lookup()](./dns.md#lookup) 的原理与其他使用相同的操作系统设备的程序一样。例如，[dns.lookup()](./dns.md#lookup) 几乎总是像 `ping` 命令一样的方式解析给定的名称。在大多数类 POSIX 的操作系统中，[dns.lookup()](./dns.md#lookup) 函数的行为可以通过改变 `nsswitch.conf(5)` 和/或 `resolv.conf(5)` 的设置进行修改，但请注意，更改这些文件将改变*同一个操作系统上运行的所有其他程序*的行为。

尽管从 JavaScript 的角度来看，调用 `dns.lookup()` 会是异步，它通过同步调用运行在 libuv 线程池中的 `getaddrinfo(3)` 实现。由于 libuv 线程池的大小是固定的，这也意味着，如果出于某种原因 `getaddrinfo(3)` 的调用花费了很长时间，其他可能在 libuv 线程池（诸如文件系统操作）中运行的操作将经历性能下降。为了缓解这一问题，一个潜在的解决方案是通过将 'UV_THREADPOOL_SIZE' 环境变量设置为一个大于 4（当前的默认值）的值来增加 libuv 线程池的大小。有关 libuv 线程池的更多信息，请参阅 [libuv 的官方文档](http://docs.libuv.org/en/latest/threadpool.html)。


<div id="resolve" class="anchor"></div>
### dns.resolve(), dns.resolve*() and dns.reverse()

这些函数与 [dns.lookup()](./dns.md#lookup) 有着完全不同的实现。它们不使用 `getaddrinfo(3)` 并且它们总是执行网络上的 DNS 查询。该网络通信始终异步进行，并且不使用 libuv 线程池。

其结果是，这些函数不能与发生在（可以有 [dns.lookup()](./dns.md#lookup) 的） libuv 线程池中的其他处理具有相同的负面影响。

它们使用与 [dns.lookup()](./dns.md#lookup) 不同的配置文件。例如，他们不使用来自 `/etc/hosts` 的配置。