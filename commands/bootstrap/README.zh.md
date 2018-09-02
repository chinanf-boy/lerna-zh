# `@lerna/bootstrap`

> 将本地包链接在一起并,安装剩余包依赖项

## 用法

```sh
$ lerna bootstrap
```

引导当前 Lerna 项目中的包. 安装所有依赖项,并链接任何交叉依赖项.

运行时,此命令将:

1.  `npm install`每个包的所有外部依赖性.
2.  将所有的相互依赖项的Lerna`packages`链接在一起.
3.  `npm run prepublish`所有引导包.
4.  `npm run prepare`所有引导包.

`lerna bootstrap`遵循`--ignore`,`--ignore-scripts`,`--scope`和`--include-filtered-dependencies`标志 ( 查阅[过滤标志](https://www.npmjs.com/package/@lerna/filter-options))

向 NPM 客户端,传递额外的参数`--`:

```sh
$ lerna bootstrap -- --production --no-optional
```

也可以配置为`lerna.json`:

```js
{
  ...
  "npmClient": "yarn",
  "npmClientArgs": ["--production", "--no-optional"]
}
```

### --hoist [glob]

在项目根中,安装匹配`glob`的外部依赖项,所以他们应用于所有的包. 这些依赖项中的任何二进制文件都会链接到依赖包中. `node_modules/.bin/`目录,所以它们可以用于 NPM 脚本. 如果选项存在,但不存在字符,`glob`默认是`**` (吊起一切吧) . 此选项仅影响`bootstrap`命令.

```sh
$ lerna bootstrap --hoist
```

`--hoist`背景介绍,见[提升文件](https://github.com/lerna/lerna/blob/master/doc/hoist.md).

注意: 如果软件包依赖于不同*版本*的外部依赖项,最常用的版本将被吊起,并发出警告.

###  --nohoist [glob]

在项目根中,*不*安装匹配`glob`的外部依赖项,这可以用于在某些依赖关系下,退出吊装.

```sh
$ lerna bootstrap --hoist --nohoist=babel-*
```

### --ignore

```sh
$ lerna bootstrap --ignore component-*
```

这个`--ignore`标志,当与`bootstrap`命令时,也可以设置在`lerna.json`的`command.bootstrap.ignore`字段. 命令行标志将优先于此选项.

**例子**

```json
{
  "version": "0.0.0",
  "command": {
    "bootstrap": {
      "ignore": "component-*"
    }
  }
}
```

> 提示: glob模式 是 与`package.json`定义的包名匹配,而不是存储在目录中的目录名.

## 选项

### `--ignore-scripts`

在引导包时,跳过正常运行的任何生命周期脚本 (`prepare`等) .

```sh
$ lerna bootstrap --ignore-scripts
```

### `--registry <url>`

当使用此标志运行时, NPM 命令将为您的包使用指定的注册表.

如果不希望在所有`package.json`文件中,单独显式地设置注册表配置 (例如,使用私有注册表) ,则这非常有用.

### `--npm-client <client>`

必须是一个知道如何安装 NPM 包依赖项的可执行文件. 默认值`--npm-client`是`npm`.

```sh
$ lerna bootstrap --npm-client=yarn
```

也可以配置为`lerna.json`:

```js
{
  ...
  "npmClient": "yarn"
}
```

### `--use-workspaces`

为了集合[Yarn 工作空间](https://github.com/yarnpkg/rfcs/blob/master/implemented/0000-workspaces-install-phase-1.md) (至yarn@0.27+) . 数组中的值是 Lerna 将把操作委托给yarn的命令 (目前仅为引导) . 如果`--use-workspaces`为true,`packages`将被`package.json/workspaces.`值覆盖.当然也可以在`lerna.json`配置:

```js
{
  ...
  "npmClient": "yarn",
  "useWorkspaces": true
}
```

根级包装. JSON 还必须包括`workspaces`数组:

```json
{
  "private": true,
  "devDependencies": {
    "lerna": "^2.2.0"
  },
  "workspaces": ["packages/*"]
}
```

这个列表与Lerna的`packages`配置相似 (一个带有 package.json 的 glob匹配目录的列表) ,除了它不支持递归所有的glob匹配 (`"**"`又名"全局星".

### `--no-ci`

使用`--npm-client`的默认值时,`lerna bootstrap`将运行[`npm ci`](https://docs.npmjs.com/cli/ci),而不是`npm install`在 CI 环境中. 若要禁用此行为,请传递`--no-ci`:

```sh
$ lerna bootstrap --no-ci
```

*强制*在本地安装期间运行 (在没有自动启用的情况下) ,通过传递`--ci`:

```sh
$ lerna bootstrap --ci
```

这可以用于"clean"重新安装,或初始安装后,初代克隆.

## 它是如何工作的

让我们使用`babel`作为一个例子.

- `babel-generator`和`source-map` (其他) 是`babel-core`的依赖项.
- `babel-core`的[`package.json`](https://github.com/babel/babel/blob/13c961d29d76ccd38b1fc61333a874072e9a8d6a/packages/babel-core/package.json#L28-L47)`dependencies`字段中列出这两个包,如下所示.

```js
// babel-core package.json
{
  "name": "babel-core",
  ...
  "dependencies": {
    ...
    "babel-generator": "^6.9.0",
    ...
    "source-map": "^0.5.0"
  }
}
```

- Lerna 检查每个依赖是否 Lerna 项目的一部分.
  - 在这个例子中,`babel-generator`可以是内部依赖,而`source-map`始终是外部依赖项.
  - 这个`babel-generator`版本在`babel-core`的`package.json`,来自l`packages/babel-generator`,属于传递内部依赖关系.
  - `source-map`是`npm install` (或`yarn`)得来的.
- `packages/babel-core/node_modules/babel-generator`链接到`packages/babel-generator`
- 这允许嵌套目录导入.

## 笔记

- 当包中的依赖版本,不被项目中的同名程序包所满足时,它将是`npm install` (或`yarn`) 得来的.
- dist标签,像`latest`不满足[semver](https://semver.npmjs.com/)规范.
- 循环依赖,导致循环链环,*可能会*影响 你的编辑器/IDE.

[Webstorm](https://www.jetbrains.com/webstorm/)在存在循环链接时,会锁上. 为了防止这一点,添加`node_modules`到忽略的文件和`Preferences | Editor | File Types | Ignored files and folders`文件夹的列表
