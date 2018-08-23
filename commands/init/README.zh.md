
# `@lerna/init`

> 创建一个新的LeNA回购协议或升级现有的回购协议到当前版本的勒纳

## 用法

```sh
$ lerna init
```

创建一个新的LeNA回购或升级现有的回购到当前版本的勒尔纳. 

> LeRNA假定回购已经被初始化. `git init`.

运行时,此命令将: 

1.  添加`lerna`作为一个[`devDependency`](https://docs.npmjs.com/files/package.json#devdependencies)在里面`package.json`如果它还不存在. 
2.  创建一个`lerna.json`配置文件以存储`version`号码. 

一个新的GIT回购的示例输出: 

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

此标志告诉LelNA使用独立的版本控制模式. 

### `--exact`

```sh
$ lerna init --exact
```

默认情况下,`lerna init`当添加或更新本地版本时,将使用插入符号范围`lerna`,就像`npm install --save-dev lerna`.

保留`lerna`1ㄡX行为的"精确"比较,通过这个标志. 它将配置`lerna.json`强制执行所有后续操作的精确匹配. 

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
