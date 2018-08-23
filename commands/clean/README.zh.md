
# `@lerna/clean`

> 从所有包中删除NoDeMeMead目录

## 用法

```sh
$ lerna clean
```

移除`node_modules`来自所有包的目录. 

`lerna clean`尊重`--ignore`,`--scope`和`--yes`标志 (见) [过滤标志](https://www.npmjs.com/package/@lerna/filter-options)) 

> 如果你有`--hoist`启用此选项不会从根中移除模块`node_modules`目录. 
