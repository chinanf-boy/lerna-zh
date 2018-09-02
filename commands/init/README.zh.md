# `@lerna/init`

> 创建一个新的 Lerna 项目 或 升级现有的项目到 Lerna 的当前版本

## 用法

```sh
$ lerna init
```

创建一个新的 Lerna 项目 或 升级现有的项目到 Lerna 的当前版本.

> Lerna 假定项目已经被`git init`初始化.

运行时,此命令将:

1.  在`package.json`中添加`lerna`作为一个[`devDependency`](https://docs.npmjs.com/files/package.json#devdependencies),如果它还不存在.
2.  创建一个`lerna.json`配置文件,以存储`version`号.

一个新的 git 项目的示例输出:

```sh
$ lerna init
lerna info version v2.0.0
lerna info Updating package.json
lerna info Creating lerna.json
lerna success Initialized Lerna files
```

## 选项

### `--independent`

```sh
$ lerna init --independent
```

此标志告诉 Lerna 使用独立的版本控制模式.

### `--exact`

```sh
$ lerna init --exact
```

默认情况下,`lerna init`当添加或更新本地版本`lerna`时,会在符号范围内,就像`npm install --save-dev lerna`.

而使用这个选项,保留`lerna` 1.x "exact"比较的行为.配置`lerna.json`,会强制匹配,所有后续的执行.

```json
{
  "command": {
    "init": {
      "exact": true
    }
  },
  "version": "0.0.0"
}
```
