
# `@lerna/exec`

> 在每个包中运行任意命令

## 用法

```sh
$ lerna exec -- <command> [..args] # runs the command in all packages
$ lerna exec -- rm -rf ./node_modules
$ lerna exec -- protractor conf.js
```

在每个包中运行任意命令. 双冲刺`--`必须将虚线标志传递给生成的命令,但当所有参数都是位置时,这是不必要的. 

当前包的名称可通过环境变量获得`LERNA_PACKAGE_NAME`: 

```sh
$ lerna exec -- npm view \$LERNA_PACKAGE_NAME
```

还可以通过环境变量在一个复杂的DIR结构中运行根目录中的脚本. `LERNA_ROOT_PATH`: 

```sh
$ lerna exec -- node \$LERNA_ROOT_PATH/scripts/some-script.js
```

## 选项

`lerna exec`尊重`--concurrency`,`--scope`和`--ignore`标志 (见) [过滤标志](https://www.npmjs.com/package/@lerna/filter-options)) 

```sh
$ lerna exec --scope my-component -- ls -la
```

> 命令是并行生成的,使用给定的并发 (除`--parallel`) 输出通过管道传输,因此不确定性. 如果要在一个包中运行命令,请使用它: 

```sh
$ lerna exec --concurrency 1 -- ls -la
```

### `--stream`

流立即从子进程输出,前缀是源程序包名. 这允许来自不同的包的输出被交织. 

```sh
$ lerna exec --stream -- babel src -d lib
```

### `--parallel`

类似`--stream`但是,完全不考虑并发性和拓扑排序,在具有前缀流输出的所有匹配包中立即运行给定的命令或脚本. 这是长期运行过程的首选标志,如`babel src -d lib -w`跑过很多包. 

```sh
$ lerna exec --parallel -- babel src -d lib -w
```

> **注: **建议在使用该命令时限制此命令的范围. `--parallel`标志,因为生成几十个子进程可能对shell的镇静 (例如,最大文件描述符限制) 有害. 牛传染性胃肠炎病毒

### `--no-bail`

```sh
# Run a command, ignoring non-zero (error) exit codes
$ lerna exec --no-bail <command>
```

默认情况下,`lerna exec`将以一个错误退出*任何*执行返回非零退出代码. 孔型`--no-bail`若要禁用此行为,请执行*全部的*包不管退出代码. 
