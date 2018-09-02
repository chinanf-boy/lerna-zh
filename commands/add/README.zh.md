# `@lerna/add`

> 向匹配的包,添加依赖项

## 用法

```sh
$ lerna add <package>[@version] [--dev] [--exact]
```

添加本地或远程`package`,作为当前 Lerna 项目中对包的依赖性.

运行时,此命令将:

1.  添加`package`到每个范围内的包. 范围的包不是`package`,而是`--scope`

> 如: 不明白, 请看[实例](#实例)

2.  引导包,修改它们的配置文件(`package.json`)

如果没有`version`提供了说明符,默认为`latest`标签,就像`npm install`.

## 选项

`lerna add`遵循`--ignore`,`--scope`和`--include-filtered-dependencies`标志 (见) [过滤标志](https://www.npmjs.com/package/@lerna/filter-options))

### `--dev`

将新软件包添加到`devDependencies`,而不是`dependencies`.

### --精确

```sh
$ lerna add --exact
```

用精确的版本添加新的包 (例如,`1.0.1`而不是`^`semver 范围 (例如,`^1.0.1`)

## 实例

```sh
# 安装 module-1 到 module-2
lerna add module-1 --scope=module-2

# 安装 module-1 到 module-2 的 devDependencies
lerna add module-1 --scope=module-2 --dev

# 安装 module-1 到 所有模块 ,除了 module-1
lerna add module-1

# 安装 babel-core 到 所有模块
lerna add babel-core
```
