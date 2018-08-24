# 常见问题

_这份文件正在进行中._

## 如何将包添加到我的 Lerna 存储库?

对于您添加到 Lerna 存储库的任何包,不是运行`npm install`,而你应该使用[`lerna bootstrap`][bootstrap]. 这将考虑到现有项目中的`packages`文件夹以及外部依赖项.

### 新包

在您的包`packages`文件夹中创建目录,并正常运行`npm init`创建`package.json`的新包.

### 现有包

你可以使用[`lerna import <package>`][import]将现有包传输到 Lerna 存储库中; 此命令将保留提交历史.

[`lerna import <package>`][import]采用本地路径而不是 URL. 在这种情况下,你需要有你想要链接到你的文件系统的repo.

[bootstrap]: https://github.com/lerna/lerna/blob/master/commands/bootstrap/README.md
[import]: https://github.com/lerna/lerna/blob/master/commands/import/README.md

## 如何重试,如果发布`publish`失败?

有时,`lerna publish`不起作用. 您的网络可能有打嗝,您可能还没有登录到 NPM 等.

如果`lerna.json`还没有更新,只是尝试一下`lerna publish`再一次.

如果已经更新,可以强制重新发布. `lerna publish --force-publish $(ls packages/)`

## bootstrap过程真的很慢,我能做什么?

里面有很多包的项目可能需要很长的时间来bootstrap.

你可以大大减少花费在`lerna bootstrap`,如果您打开hoist,请参见[hoist文件](./doc/hoist.md)了解更多信息.

与[使用yarn作为 NPM 客户端](https://github.com/lerna/lerna#--npm-client-client)而不是`npm`的方法相结合,您可以更进一步地提高bootstrap性能. .

## 根目录的`package.json`

这根`package.json`,至少,是你在CI构建期间在本地安装`lerna`的方法. 您还应该把 测试,linting 和 类似的任务放在那里,以便从根目录运行它们,因为从每个包单独运行它们比较慢. 根还可以保存所有"hoisted"的包,这帮带[`--hoist`][hoist]参数的bootstapping加速

可以将 根目录 作为托管位置添加 (在`lerna.json`的`packages`字段数组)-如果你需要. 这将导致 Lerna 将 根依赖关系 链接到 包们 的目录,运行`postinstall`脚本与其他,等等.

[hoist]: https://github.com/lerna/lerna/blob/master/doc/hoist.md

## CI 设置

如上所述,根`package.json`负责局部安装`lerna`. 不过你需要自动化`bootstrap`. 这可以通过将它作为 NPM 脚本,在 CI 阶段使用.

实例 根`package.json`:

```json
{
  "name": "my-monorepo",
  "private": true,
  "devDependencies": {
    "eslint": "^3.19.0",
    "jest": "^20.0.4",
    "lerna": "^2.0.0"
  },
  "scripts": {
    "bootstrap": "lerna bootstrap --hoist",
    "pretest": "eslint packages",
    "test": "jest"
  }
}
```

CircleCI 的配置文件示例(`circle.yml`) :

```yml
dependencies:
  post:
    - npm run bootstrap
```
