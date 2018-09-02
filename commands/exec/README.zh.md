# `@lerna/exec`

> 在每个包中运行任意命令

## 用法

```sh
$ lerna exec -- <command> [..args] # 运行命令在
$ lerna exec -- rm -rf ./node_modules
$ lerna exec -- protractor conf.js
```

在每个包中运行任意命令. 双破折号`--`的虚线标志传递 给 将要运行的命令,但当所有参数都是位置时,这是不必要的.

当前包的名称,可通过环境变量`LERNA_PACKAGE_NAME`获得:

```sh
$ lerna exec -- npm view \$LERNA_PACKAGE_NAME
```

还可以通过环境变量`LERNA_ROOT_PATH`,在一个复杂的 dir结构中 运行根目录中的脚本. :

```sh
$ lerna exec -- node \$LERNA_ROOT_PATH/scripts/some-script.js
```

## 选项

`lerna exec`遵循`--concurrency`,`--scope`和`--ignore`标志 ( 查阅[过滤标志](https://www.npmjs.com/package/@lerna/filter-options))

```sh
$ lerna exec --scope my-component -- ls -la
```

> 命令是并行生成的,使用给定的并发 (除了`--parallel`) 输出,管道传输,因此具有不确定性. 如果要在一个包中运行命令,请使用它:

```sh
$ lerna exec --concurrency 1 -- ls -la
```

### `--stream`

输出流立即从子进程输出,输出的前缀是源程序包名. 这允许来自不同的包的输出交织在一起.

```sh
$ lerna exec --stream -- babel src -d lib
```

### `--parallel`

类似`--stream`但是,完全不考虑并发性和拓扑排序,在具有前缀流输出的所有匹配包中,立即运行给定的命令或脚本. 这是长期运行过程的首选标志,如在很多包上运行`babel src -d lib -w`.

```sh
$ lerna exec --parallel -- babel src -d lib -w
```

> **注:** 建议在使用该`--parallel`标志命令时限制此命令的范围. ,因为生成几十个子进程可能对 shell 的显示 (例如,最大文件描述符限制) 有害. 例如: YMMV

### `--no-bail`

```sh
# Run a command, ignoring non-zero (error) exit codes
$ lerna exec --no-bail <command>
```

默认情况下,`lerna run`若有一个错误,就退出*任何*脚本运行,返回非零退出代码. 传递`--no-bail`若要禁用此行为,无论退出代码如何，都在包含它的_全部_包中运行脚本
