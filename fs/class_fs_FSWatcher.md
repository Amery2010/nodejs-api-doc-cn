# fs.FSWatcher类

* ['change' 事件](#event_change)
* ['error' 事件](#event_error)
* [watcher.close()](#close)

--------------------------------------------------


从 `fs.watch()` 返回的对象是该类型。


<div id="event_change" class="anchor"></div>
## 'change' 事件

* `event` {String} fs 的类型变化。

* `filename` {String} 更改的文件名（如果相关/可用）

在监视的目录或文件更改了一些东西时触发。在 [fs.watch()](./fs.md#watch) 中了解更多内容。


<div id="event_error" class="anchor"></div>
## 'error' 事件

* `error` {Error}

当发生错误时触发。


<div id="close" class="anchor"></div>
## watcher.close()

对给定的 `fs.FSWatcher` 停止监视变化。