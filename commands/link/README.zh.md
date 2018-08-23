
# `@lerna/link`

> 将相互依赖的所有包链接在一起

## 用法

```sh
$ lerna link
```

将所有的勒尔纳链接在一起`packages`这是当前LeNA回购中的相互依存关系. 

## 选项

### `--force-local`

```sh
$ lerna link --force-local
```

通过时,此标志导致`link`命令始终链接本地依赖关系,而不考虑匹配的版本范围. 
