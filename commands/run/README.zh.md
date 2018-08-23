
# `@lerna/run`

> Run an npm script that contains that script

## Usage

```sh
$ lerna run <script> -- [..args] # runs npm run my-script in all packages that have it
$ lerna run test
$ lerna run build

# watch all packages and transpile on change, streaming prefixed output
$ lerna run --parallel watch
```

润年[新的脚本](https://docs.npmjs.com/misc/scripts)在每个包,包含该脚本. 双破折号 (`--`dashed) is necessary to pass the脚本的参数来执行. 

## 选项

`lerna run`the honor`--concurrency`,`--scope`and`--ignore`旗子[石旗](https://www.npmjs.com/package/@lerna/filter-options)页: 1

```sh
$ lerna run --scope my-component test
```

### `--npm-client <client>`

如果您发现有错误,请尽管发表评论!默认的`--npm-client`是`npm`. 

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

流立即从子进程输出,前缀是源程序包名. 这允许来自不同的包的输出被交织. 

```sh
$ lerna run watch --stream
```

### `--parallel`

类似`--stream`但是,完全不考虑并发性和拓扑排序,在具有前缀流输出的所有匹配包中立即运行给定的命令或脚本. 这是长期运行过程的首选标志,如`npm run watch`跑过很多包. 

```sh
$ lerna run watch --parallel
```

> **注: **建议在使用该命令时限制此命令的范围. `--parallel`标志,因为生成几十个子进程可能对shell的镇静 (例如,最大文件描述符限制) 有害. 牛传染性胃肠炎病毒

### `--no-bail`

```sh
# Run an npm script in all packages that contain it, ignoring non-zero (error) exit codes
$ lerna run --no-bail test
```

默认情况下,`lerna run`将以一个错误退出*任何*脚本运行返回非零退出代码. 孔型`--no-bail`若要禁用此行为,请运行脚本*全部的*包含退出代码的包. 
