# `@lerna/clean`

> 从所有包中删除 node_modules 目录

## 用法

```sh
$ lerna clean
```

从所有包的目录,移除`node_modules`目录.

`lerna clean`遵循`--ignore`,`--scope`和`--yes`参数选项 (见) [过滤选项](https://www.npmjs.com/package/@lerna/filter-options))

> 如果你启用`--hoist`选项,那么将不会从根`node_modules`目录中移除模块.
