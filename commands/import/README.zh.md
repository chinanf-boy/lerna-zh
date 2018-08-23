
# `@lerna/import`

> 使用提交历史将包导入到MMOREPO中

## 用法

```sh
$ lerna import <path-to-external-repository>
```

导入包装`<path-to-external-repository>`具有提交历史的`packages/<directory-name>`. 保留原始提交作者ㄡ日期和消息. 提交应用于当前分支. 

这对于收集预先存在的独立软件包到LeNA回购是有用的. 修改每个提交以相对于包目录进行更改. 例如,添加的提交`package.json`而是将添加`packages/<directory-name>/package.json`.

## 选项

### `--flatten`

当导入具有合并提交的存储库时,导入命令将无法应用所有提交. 用户可以使用此标志来请求导入"平面"历史,即每次合并提交作为合并引入的单个更改. 

    $ lerna import ~/Product --flatten
