
# 常见问题

*这份文件正在进行中. *

## 如何将包添加到我的LeRNA存储库?

对于您添加到LeNA存储库的任何包,而不是运行`npm install`你应该跑步[`lerna bootstrap`][bootstrap]. 这将考虑到现有项目中的`packages`文件夹以及外部依赖项. 

### 新包装

在您的包中创建目录`packages`文件夹,并运行`npm init`正常创建`package.json`你的新包裹. 

### 现有包

你可以使用[`lerna import <package>`][import]将现有包传输到Lerna存储库中;此命令将保留提交历史. 

[`lerna import <package>`][import]采用本地路径而不是URL. 在这种情况下,你需要有你想要链接到你的文件系统的回购. 

[bootstrap]: https://github.com/lerna/lerna/blob/master/commands/bootstrap/README.md

[import]: https://github.com/lerna/lerna/blob/master/commands/import/README.md

## 如何重试发布`publish`失败?

有时,`lerna publish`不起作用. 您的网络可能有打嗝,您可能还没有登录到NPM等. 

如果`lerna.json`还没有更新,只是尝试一下`lerna publish`再一次. 

如果已经更新,可以强制重新发布. `lerna publish --force-publish $(ls packages/)`

## 引导过程真的很慢,我能做什么?

里面有很多包的项目可能需要很长的时间来引导. 

你可以大大减少花费在`lerna bootstrap`如果您打开吊装,请参见[吊装文件](./doc/hoist.md)欲了解更多信息. 

与此相结合,您可以更进一步地提高引导性能. [使用纱线作为NPM客户机](https://github.com/lerna/lerna#--npm-client-client)而不是`npm`.

## 根`package.json`

根`package.json`至少,你是如何安装的`lerna`在CI构建过程中本地进行. 您还应该把测试ㄡ内联和类似的任务放在那里,以便从根目录运行它们,因为从每个包单独运行它们比较慢. 根还可以保存所有"挂起"的包,这加速了使用时的自举. [`--hoist`][hoist]旗帜. 

可以将根作为托管位置添加 (在`packages`数组`lerna.json`-如果这是你需要的这将导致LeRNA将根依赖关系链接到包的目录,运行. `postinstall`脚本与其他,等等. 

[hoist]: https://github.com/lerna/lerna/blob/master/doc/hoist.md

## CI设置

如上所述根`package.json`负责安装`lerna`局部地. 你需要自动化`bootstrap`不过. 这可以通过将它作为NPM脚本在CI阶段使用来实现. 

实例根`package.json`: 

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

CircleCI的配置文件示例`circle.yml`) : 

```yml
dependencies:
  post:
    - npm run bootstrap
```
