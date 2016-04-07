# 附录：包管理技巧

Node.js 的 `require()` 函数的语义被设计的足够通用化，可以支持各种常规目录结构。包管理程序如 `dpkg`、`rpm` 和 `npm` 将不用修改就能够从 Node.js 模块构建本地包。

接下来我们将给你一个可行的目录结构建议：

假设我们希望将一个包的指定版本放在 `/usr/lib/node/<some-package>/<some-version>` 目录中。

包可以依赖于其他包。为了安装包 `foo` ，可能需要安装包 `bar` 的一个指定版本。 包 `bar` 也可能有依赖关系，在某些情况下依赖关系可能发生冲突或形成循环。

因为 Node.js 会查找它所加载的模块的真实路径（也就是说会解析符号链接），然后按照[上文描述的方式](./loading_from_node_modules_folders.md#)在 `node_modules` 目录中查询依赖关系，这种情形跟以下体系结构非常相似：

- `/usr/lib/node/foo/1.2.3/` - `foo` 包 1.2.3 版本的内容

- `/usr/lib/node/bar/4.3.2/` - `foo` 包所依赖的 `bar` 包的内容

- `/usr/lib/node/foo/1.2.3/node_modules/bar` - 指向 `/usr/lib/node/bar/4.3.2/` 的符号链接

- `/usr/lib/node/bar/4.3.2/node_modules/*` - 指向 `bar` 包所依赖的包的符号链接

因此，即便存在循环依赖或依赖冲突，每个模块还是可以获得它所依赖的包的一个可用版本。

当 `foo` 包中的代码调用 `require('bar')` ，将获得符号链接 `/usr/lib/node/foo/1.2.3/node_modules/bar` 指向的版本。 然后，当 `bar` 包中的代码调用 `require('queue')` ，将会获得符号链接 `/usr/lib/node/bar/4.3.2/node_modules/quux` 指向的版本。

此外，为了进一步优化模块搜索过程，不要将包直接放在 `/usr/lib/node` 目录中，而是将它们放在 `/usr/lib/node_modules/<name>/<version>` 目录中。 这样在找不到依赖包的情况下，Node.js 就不会在 `/usr/node_modules` 或 `/node_modules` 目录中查找了。

为了使模块在 Node.js 的 REPL 中可用，你可能需要将 `/usr/lib/node_modules` 目录加入到 `$NODE_PATH`
环境变量中。由于在 `node_modules` 目录中搜索模块使用的是相对路径，使得调用 `require()` 获得的是基于真实路径的文件，因此包本身可以放在任何位置。