# `@lerna/list`

> 列出本地包

## 用法

这个`list`子命令被弄成几个方便的短命令 (类似于[`npm ls`](https://docs.npmjs.com/cli/ls)) :

- `lerna ls`相同的`lerna list`,它本身类似于`ls`命令
- `lerna ll`相当于`lerna ls -l`,显示[长的](#--long)输出
- `lerna la`相当于`lerna ls -la`,显示[全部的](#--all)包装 (包括私人包装)

```sh
$ lerna ls
package-1
package-2
```

当在 shell 中运行这些命令时,您可能会注意到`lerna`额外的日志记录. 放心,他们不会感染你的管道咒语,因为所有的日志都发射到`stderr`而不是`stdout`.

无论如何,你总能通过`--loglevel silent`创造神奇的shell魔法链.

## 选项

- [`--json`](#--json)
- [`-a`,`--all`](#--all)
- [`-l`,`--long`](#--long)
- [`-p`,`--parseable`](#--parseable)

`lerna ls`也遵循所有可用[过滤标志](https://www.npmjs.com/package/@lerna/filter-options).

### `--json`

将信息显示为 JSON 数组.

```sh
$ lerna ls --json
[
  {
    "name": "package-1",
    "version": "1.0.0",
    "private": false,
    "location": "/path/to/packages/pkg-1"
  },
  {
    "name": "package-2",
    "version": "1.0.0",
    "private": false,
    "location": "/path/to/packages/pkg-2"
  }
]
```

**提示:** 管道到[`json`](http://trentm.com/json/)实用工具,可用于选择单个属性:

```sh
$ lerna ls --json --all | json -a -c 'this.private === true' name
package-3
```

### `--all`

别名: `-a`

显示默认隐藏的私有包.

```sh
$ lerna ls --all
package-1
package-2
package-3 (private)
```

### `--long`

别名: `-l`

显示扩展信息.

```sh
$ lerna ls --long
package-1 v1.0.1 packages/pkg-1
package-2 v1.0.2 packages/pkg-2

$ lerna ls -la
package-1 v1.0.1 packages/pkg-1
package-2 v1.0.2 packages/pkg-2
package-3 v1.0.3 packages/pkg-3 (private)
```

### `--parseable`

别名: `-p`

显示可解析输出,而不是圆柱视图.

默认情况下,输出的每一行都是一个包的绝对路径.

在`--long`输出,每行用 `:` - 分隔列表: `<fullpath>:<name>:<version>[:flags..]`

```sh
$ lerna ls --parseable
/path/to/packages/pkg-1
/path/to/packages/pkg-2

$ lerna ls -pl
/path/to/packages/pkg-1:package-1:1.0.1
/path/to/packages/pkg-2:package-2:1.0.2

$ lerna ls -pla
/path/to/packages/pkg-1:package-1:1.0.1
/path/to/packages/pkg-2:package-2:1.0.2
/path/to/packages/pkg-3:package-3:1.0.3:PRIVATE
```
