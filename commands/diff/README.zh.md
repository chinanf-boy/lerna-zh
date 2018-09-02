
# `@lerna/diff`

> 自上次发布以来的所有包 或 单个包

## 用法

```sh
$ lerna diff [package]

$ lerna diff
# diff a specific package
$ lerna diff package-name
```

自上次发布以来,所有的包或单个包. 

> 类似`lerna changed`. 这个命令会运行`git diff`.
