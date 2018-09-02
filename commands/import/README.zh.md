# `@lerna/import`

> 将包导入到 monorepo 中, 通过提交历史

## 用法

```sh
$ lerna import <path-to-external-repository>
```

导入`<path-to-external-repository>`包到 具有提交历史的`packages/<directory-name>`. 保留原始提交作者的日期和消息. 提交应用于当前分支.

这对于集合**预先存在的独立软件包**到 Lerna 项目是有用的. 每个修改相对于包目录的提交. 例如,添加提交到`package.json`,而不添加到`packages/<directory-name>/package.json`.

## 选项

### `--flatten`

当导入具有合并提交的存储库时,导入命令将无法应用所有提交. 用户可以使用此标志来请求导入"flatten"历史,即每次合并提交, 作为单个合并的更改.

    $ lerna import ~/Product --flatten
