
# `@lerna/add`

> 向匹配的包添加依赖项

## 用法

```sh
$ lerna add <package>[@version] [--dev] [--exact]
```

添加本地或远程`package`作为当前LeNA回购中对包的依赖性. 

运行时,此命令将: 

1.  添加`package`对每个适用的包. 适用的不是包装`package`在范围之内
2.  具有清单文件更改的自举包`package.json`) 

如果没有`version`提供了说明符,默认为`latest`DIST标签,就像`npm install`.

## 选项

`lerna add`尊重`--ignore`,`--scope`和`--include-filtered-dependencies`标志 (见) [过滤标志](https://www.npmjs.com/package/@lerna/filter-options)) 

### `--dev`

将新软件包添加到`devDependencies`而不是`dependencies`.

### --精确

```sh
$ lerna add --exact
```

用精确的版本添加新的包 (例如,`1.0.1`而不是违约`^`SSEVER范围 (例如,`^1.0.1`) 

## 实例

```sh
# Install module-1 to module-2
lerna add module-1 --scope=module-2

# Install module-1 to module-2 in devDependencies
lerna add module-1 --scope=module-2 --dev

# Install module-1 in all modules except module-1
lerna add module-1

# Install babel-core in all modules
lerna add babel-core
```
