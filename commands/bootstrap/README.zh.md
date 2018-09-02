# `@lerna/bootstrap`

> 将本地包链接在一起并安装剩余包依赖项

## 用法

```sh
$ lerna bootstrap
```

引导当前 Lerna 项目中的包. 安装所有依赖项并链接任何交叉依赖项.

运行时,此命令将:

1.  `npm install`每个包的所有外部依赖性.
2.  将所有的勒尔纳链接在一起`packages`这是相互依存的关系.
3.  `npm run prepublish`在所有自举包中.
4.  `npm run prepare`在所有自举包中.

`lerna bootstrap`遵循`--ignore`,`--ignore-scripts`,`--scope`和`--include-filtered-dependencies`标志 (见) [过滤标志](https://www.npmjs.com/package/@lerna/filter-options))

向 NPM 客户端传递额外的参数`--`:

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

### ℴℴ 提升机[格洛布]

安装外部依赖项匹配`glob`在项目根,所以他们可以对所有的包. 这些依赖项中的任何二进制文件都会链接到依赖包中. `node_modules/.bin/`目录,所以它们可以用于 NPM 脚本. 如果选项存在但不存在`glob`默认是`**` (吊起一切) . 此选项仅影响`bootstrap`命令.

```sh
$ lerna bootstrap --hoist
```

背景介绍`--hoist`见[提升文件](https://github.com/lerna/lerna/blob/master/doc/hoist.md).

注意: 如果软件包依赖于不同的*版本*对于外部依赖项,最常用的版本将被吊起,并发出警告.

### ℴℴ 诺霍斯特[格洛布]

做*不*安装外部依赖项匹配`glob`在项目的根源上. 这可以用于在某些依赖关系下退出吊装.

```sh
$ lerna bootstrap --hoist --nohoist=babel-*
```

### --忽略

```sh
$ lerna bootstrap --ignore component-*
```

这个`--ignore`标志,当与`bootstrap`命令,也可以设置在`lerna.json`下`command.bootstrap.ignore`关键. 命令行标志将优先于此选项.

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

> 提示: GLUB 与定义的包名匹配`package.json`而不是存储在目录中的目录名.

## 选项

### `--ignore-scripts`

跳过正常运行的任何生命周期脚本 (`prepare`等) 在自举包中.

```sh
$ lerna bootstrap --ignore-scripts
```

### `--registry <url>`

当使用此标志运行时,转发的 NPM 命令将为您的包使用指定的注册表.

如果不希望在所有 package.json 文件中单独显式地设置注册表配置 (例如,使用私有注册表) ,则这非常有用.

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

使能与[纱线工作空间](https://github.com/yarnpkg/rfcs/blob/master/implemented/0000-workspaces-install-phase-1.md) (可从纱线@ 0.27 +) . 数组中的值是勒纳将把操作委托给纱线的命令 (目前仅为引导) . 如果`--use-workspaces`那么是真的`packages`将被值从`package.json/workspaces.`也可以配置为`lerna.json`:

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

这个列表与勒纳的相似. `packages`CONFIG (一个带有 Copy.jSON 的 GLUX 匹配目录的列表) ,除了它不支持递归 GLUS (`"**"`又名"环球星".

### `--no-ci`

使用默认值时`--npm-client`,`lerna bootstrap`将呼叫[`npm ci`](https://docs.npmjs.com/cli/ci)而不是`npm install`在 CI 环境中. 若要禁用此行为,请通过`--no-ci`:

```sh
$ lerna bootstrap --no-ci
```

到*力*在本地安装期间 (在没有自动启用的情况下) `--ci`:

```sh
$ lerna bootstrap --ci
```

这可以用于"干净"重新安装,或初始安装后,新鲜克隆.

## 它是如何工作的

让我们使用`babel`作为一个例子.

- `babel-generator`和`source-map` (其他) 是依赖关系`babel-core`.
- `babel-core`的[`package.json`](https://github.com/babel/babel/blob/13c961d29d76ccd38b1fc61333a874072e9a8d6a/packages/babel-core/package.json#L28-L47)列出这两个包作为密钥`dependencies`,如下所示.

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

- Lerna 检查每个依赖也是 Lerna 项目的一部分.
  - 在这个例子中,`babel-generator`可以是内部依赖,而`source-map`始终是外部依赖项.
  - 版本`babel-generator`在`package.json`属于`babel-core`满意`packages/babel-generator`传递内部依赖关系.
  - `source-map`是`npm install`ED (或`yarn`ED) 正常.
- `packages/babel-core/node_modules/babel-generator`链接到`packages/babel-generator`
- 这允许嵌套目录导入.

## 笔记

- 当包中的依赖版本不被项目中的同名程序包所满足时,它将是`npm install`ED (或`yarn`ED) 正常.
- DIST 标签,像`latest`不满足[塞弗](https://semver.npmjs.com/)范围.
- 循环依赖导致循环链环*可以*影响编辑/ IDE.

[网络风暴](https://www.jetbrains.com/webstorm/)当存在循环链接时锁定. 为了防止这一点,添加`node_modules`到忽略的文件和文件夹的列表中`Preferences | Editor | File Types | Ignored files and folders`.
