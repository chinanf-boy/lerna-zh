# `@lerna/run`

> 如模块包中具有对应脚本,则运行npm script

## Usage

```sh
$ lerna run <script> -- [..args] # 运行 npm script 在所欲包中, 如果包中有这个脚本
$ lerna run test
$ lerna run build

# 观看所有软件包并进行变更，并带有包名前缀,输出
$ lerna run --parallel watch
```

运行[npm 脚本](https://docs.npmjs.com/misc/scripts)在每个在包含该脚本的每个包中. 需要双破折号（`--`）将虚线参数传递给脚本执行。

## 选项

`lerna run`遵循 `--concurrency`,`--scope`和`--ignore`标志(查阅[过滤参数选项](https://www.npmjs.com/package/@lerna/filter-options))

```sh
$ lerna run --scope my-component test
```

### `--npm-client <client>`

必须是知道如何运行npm生命周期脚本的可执行文件! 默认的`--npm-client`是`npm`.

```sh
$ lerna run build --npm-client=yarn
```

也可以配置为`lerna.json`:

```json
{
  "command": {
    "run": {
      "npmClient": "yarn"
    }
  }
}
```

### `--stream`

输出流立即从子进程输出,输出的前缀是源程序包名. 这允许来自不同的包的输出交织在一起.

```sh
$ lerna run watch --stream
```

### `--parallel`

类似`--stream`但是,完全不考虑并发性和拓扑排序,在具有前缀输出流的所有匹配包中立即运行给定的命令或脚本. 这是长期运行过程的首选标志,如很多包上都运行`npm run watch`.

```sh
$ lerna run watch --parallel
```

> **注: **建议在使用该`--parallel`标志命令时限制此命令的范围. ,因为生成几十个子进程可能对 shell 的显示 (例如,最大文件描述符限制) 有害. 例如: YMMV

### `--no-bail`

```sh
# Run an npm script in all packages that contain it, ignoring non-zero (error) exit codes
$ lerna run --no-bail test
```

默认情况下,`lerna run`若有一个错误,就退出*任何*脚本运行,返回非零退出代码. 传递`--no-bail`若要禁用此行为,无论退出代码如何，都在包含它的_全部_包中运行脚本
