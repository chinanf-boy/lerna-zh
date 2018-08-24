# lerna [![explain]][source] [![translate-svg]][translate-list]

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
    
「 用于 管理 拥有多个packages 的 js项目 的工具 」

> 比如 `npm install -g @vue/cli` ==> 这样的`@vue`样式, 供给 vuejs组织 管理 [cli生态链工具](https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue)
> 或插件,比如[gatsbyjs](https://github.com/gatsbyjs/gatsby/tree/master/packages)

[中文](./readme.md) | [english](https://github.com/lerna/lerna)


---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'lerna/lerna' -->
<!-- commit = '2760306a68183fcaffa4a0e4d786ce4e46956f62' -->
<!-- time = '2018 8.18' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018 8.18 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/lerna/lerna.svg
[commit]: https://github.com/lerna/lerna/tree/2760306a68183fcaffa4a0e4d786ce4e46956f62

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<p align="center">
  <img alt="Lerna" src="https://cloud.githubusercontent.com/assets/952783/15271604/6da94f96-1a06-11e6-8b04-dc3171f79a90.png" width="480">
</p>

<p align="center">
  用于 管理 拥有多个packages 的 js项目 的工具.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/lerna"><img alt="NPM Status" src="https://img.shields.io/npm/v/lerna.svg?style=flat"></a>
  <a href="https://travis-ci.org/lerna/lerna"><img alt="Travis Status" src="https://img.shields.io/travis/lerna/lerna/master.svg?style=flat&label=travis"></a>
  <a href="https://ci.appveyor.com/project/lerna/lerna/branch/master"><img alt="Appveyor Status" src="https://img.shields.io/appveyor/ci/lerna/lerna/master.svg"></a>
  <a href="https://slack.lernajs.io/"><img alt="Slack Status" src="https://slack.lernajs.io/badge.svg"></a>
</p>


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [关于](#%E5%85%B3%E4%BA%8E)
  - [Lerna项目是什么样的?](#lerna%E9%A1%B9%E7%9B%AE%E6%98%AF%E4%BB%80%E4%B9%88%E6%A0%B7%E7%9A%84)
  - [Lerna能做什么?](#lerna%E8%83%BD%E5%81%9A%E4%BB%80%E4%B9%88)
- [入门](#%E5%85%A5%E9%97%A8)
- [怎么运行的](#%E6%80%8E%E4%B9%88%E8%BF%90%E8%A1%8C%E7%9A%84)
  - [固定/锁定模式 (默认)](#%E5%9B%BA%E5%AE%9A%E9%94%81%E5%AE%9A%E6%A8%A1%E5%BC%8F-%E9%BB%98%E8%AE%A4)
  - [独立模式 (`--independent`)](#%E7%8B%AC%E7%AB%8B%E6%A8%A1%E5%BC%8F---independent)
- [故障排除](#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [经常问的问题](#%E7%BB%8F%E5%B8%B8%E9%97%AE%E7%9A%84%E9%97%AE%E9%A2%98)
- [概念](#%E6%A6%82%E5%BF%B5)
  - [lerna.json](#lernajson)
  - [共拥`devDependencies`](#%E5%85%B1%E6%8B%A5devdependencies)
  - [Git 托管依赖](#git-%E6%89%98%E7%AE%A1%E4%BE%9D%E8%B5%96)
  - [readme徽章](#readme%E5%BE%BD%E7%AB%A0)
  - [巫法](#%E5%B7%AB%E6%B3%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

-   命令
    - [ ]  [`lerna publish`](./commands/publish#readme)
    - [ ]  [`lerna version`](./commands/version#readme)
    - [ ]  [`lerna bootstrap`](./commands/bootstrap#readme)
    - [ ]  [`lerna list`](./commands/list#readme)
    - [ ]  [`lerna changed`](./commands/changed#readme)
    - [ ]  [`lerna diff`](./commands/diff#readme)
    - [ ]  [`lerna exec`](./commands/exec#readme)
    - [ ]  [`lerna run`](./commands/run#readme)
    - [ ]  [`lerna init`](./commands/init#readme)
    - [ ]  [`lerna add`](./commands/add#readme)
    - [ ]  [`lerna clean`](./commands/clean#readme)
    - [ ]  [`lerna import`](./commands/import#readme)
    - [ ]  [`lerna link`](./commands/link#readme)

## 关于

大型的代码库 拆分为 独立的版本包 对 代码共享非常有用,然而,要对这么 多的存储库进行更改是如此凌乱且难以追踪，还有还有 跨存储库测试 也变得非常复杂。

为了解决这样 (那样) 的问题, 一些项目组会将他们的基础代码库分组成多个包库中 (有时称为[monorepos](https://github.com/babel/babel/blob/master/doc/design/monorepo.md)) . 项目如[babel](https://github.com/babel/babel/tree/master/packages),[react](https://github.com/facebook/react/tree/master/packages),[angular](https://github.com/angular/angular/tree/master/modules),[ember](https://github.com/emberjs/ember.js/tree/master/packages),[meteor](https://github.com/meteor/meteor/tree/devel/packages),[jest](https://github.com/facebook/jest/tree/master/packages),和 其他项目组开发他们的包都是在一个repo库.


**Lerna是一个优化了git和npm管理多包存储库的工作流程的工具.**

Lerna还可以减少开发和构建环境中大量软件包副本的时间和空间的大小 - 通常是将项目划分为多个单独的NPM软件包的缺点. 见[提升文件](doc/hoist.zh.md)详情. 

### Lerna项目是什么样的?

实际上它真的很小小. 您有一个如下所示的文件系统: 

    my-lerna-repo/
      package.json
      packages/
        package-1/
          package.json
        package-2/
          package.json

### Lerna能做什么?

Lerna的两个主要命令是`lerna bootstrap`和`lerna publish`. 

- `bootstrap`将把repo中的依赖关系链接在一起. 
- `publish`将帮助发布任何更新的包. 

## 入门

> 以下说明适用于Lerna 3.x.对于新的Lerna项目,我们建议使用它而不是2.x版本.

让我们首先安装Lerna作为项目的dev依赖项, 通过[npm](https://www.npmjs.com/). 

```sh
$ mkdir lerna-repo && cd $_
$ npx lerna init
```

这将创建一个`lerna.json`配置文件以及`packages`文件夹,所以你的文件夹,现在应该是这样的: 

    lerna-repo/
      packages/
      package.json
      lerna.json

## 怎么运行的

Lerna允许您使用以下两种模式之一来管理项目: 固定或独立. 

### 固定/锁定模式 (默认) 

固定模式的Lerna项目在单一的版本线上运行. 版本保存在位于项目的根目录下`lerna.json`文件的`version`字段. 当你运行`lerna publish`,自上次发布版本以来,如果模块已更新,它将更新为您要发布的新版本. 这意味着您需要发布包的新版本, 就可以`publish`. 

这是[babel](https://github.com/babel/babel)目前正在使用的模式. 如果要自动将所有包的版本绑定在一起,请使用此选项. 这种方法的一个问题是任何包中的重大更改,都将导致所有包更新为新的主要版本. 

### 独立模式 (`--independent`) 

独立模式的Lerna项目允许维护人员相互独立地增加包版本. 每次发布时,您都会收到每个已更改的包的提示,以指定它是patch,minor,major还是自定义更改. 

独立模式允许您更具体地更新每个包的版本,并赋予一组组件意义. 将此模式与类似[semantic-release](https://github.com/semantic-release/semantic-release)结合起来会减少痛苦.  (目前已有[Atlassian/lerna-semantic-release](https://github.com/atlassian/lerna-semantic-release)在工作) . 

> 在独立模式下`lerna.json`的`version`字段被忽略. 

## 故障排除

- [x] 如果您在使用Lerna时遇到任何问题,请查看我们的[故障排除](doc/troubleshooting.zh.md)文档,上面记录您可能找到的问题答案. 

## 经常问的问题

- [x] 看[FAQ.zh.md](FAQ.zh.md). 

## 提示

Lerna将记录`lerna-debug.log`文件 (像`npm-debug.log`) ,当遇到运行命令的错误时. 

Lerna也有支持[scoped 包](https://docs.npmjs.com/misc/scope). 

运行`lerna`没有参数,将显示所有命令/选项. 

### lerna.json

```json
{
  "version": "1.1.3",
  "command": {
    "publish": {
      "ignoreChanges": [
        "ignored-file",
        "*.md"
      ]
    },
    "bootstrap": {
      "ignore": "component-*",
      "npmClientArgs": ["--no-package-lock"]      
    }
  },
  "packages": ["packages/*"]
}
```

-   `version`: 存储库的当前版本. 
-   `command.publish.ignoreChanges`: 一组匹配模式,将不包括在`lerna changed/publish`内. 使用此选项可防止不必要地发布新版本,例如进行更改修复`README.md`错字. 
-   `command.bootstrap.ignore`: 一组匹配模式,在运行`lerna bootstrap`命令时不会被引导. 
-   `command.bootstrap.npmClientArgs`: 字符串数组,直接传递给`npm install`,作为参数. 在`lerna bootstrap`命令期间. 
-   `command.bootstrap.scope`: 一组匹配模式,限制运行时引导哪些包,在`lerna bootstrap`命令期间. 
-   `packages`: 用作包位置的匹配模式数组. 

`lerna.json`的包配置,匹配包含`package.json`的目录列表,这正是`lerna`识别它的"leaf"-枝叶包的方法.  (vs "根"的package.json,用于管理整个repo的dev依赖项和脚本) . 

默认情况下,lerna将包列表初始化为`["packages/*"]`,但你也可以使用另一个目录,如`["modules/*"]`, 要么`["package1", "package2"]`. 定义的模式是相对于`lerna.json`所在的目录,通常是存储库根目录. 唯一的限制是您不能直接嵌套包位置,但这也是与"普通"npm包共享的限制一样. 

例如,`["packages/*", "src/**"]`匹配这棵树: 

    packages/
    ├── foo-pkg
    │   └── package.json
    ├── bar-pkg
    │   └── package.json
    ├── baz-pkg
    │   └── package.json
    └── qux-pkg
        └── package.json
    src/
    ├── admin
    │   ├── my-app
    │   │   └── package.json
    │   ├── stuff
    │   │   └── package.json
    │   └── things
    │       └── package.json
    ├── profile
    │   └── more-things
    │       └── package.json
    ├── property
    │   ├── more-stuff
    │   │   └── package.json
    │   └── other-things
    │       └── package.json
    └── upload
        └── other-stuff
            └── package.json

叶包在`packages/*`的下面,被认为是"最佳实践",但`Lerna`不要求一定如此使用. 

### 共拥`devDependencies`

大多数`devDependencies`可以拉到Lerna包的根目录.

这有一些好处: 

-   所有包都使用给定依赖项的相同版本
-   使用自动化工具可以使根目录的依赖关系保持最新状态[GreenKeeper](https://greenkeeper.io/)
-   依赖安装时间减少
-   需要更少的存储空间

**注意:**`devDependencies`提供给 npm scripts 使用的"二进制"可执行文件,仍然需要直接安装在每个使用它们的包中. 

比如说`nsp`在`lerna run nsp`情况下,依赖是必要的 (和`npm run nsp`在包的目录内) 才能正常工作: 

```json
{
  "scripts": {
    "nsp": "nsp"
  },
  "devDependencies": {
    "nsp": "^2.3.3"
  }
}
```

### Git 托管依赖

Lerna允许将 **本地依赖包的目标版本** 编写为[git remote url](https://docs.npmjs.com/cli/install),与`committish` (例如. ,`#v1.0.0`要么`#semver:^1.0.0`) ,而不是正常的数字版本范围. 这允许私有包的分发,通过 git存储库,而[私人npm注册表是不可取的](https://www.dotconferences.com/2016/05/fabien-potencier-monolithic-repositories-vs-many-repositories). 

请注意lerna*并不*执行,将git历史记录实际拆分到单独的只读存储库中. 这是用户的责任.  (看到[这个评论](https://github.com/lerna/lerna/pull/1033#issuecomment-335894690)的实现细节) 

    // packages/pkg-1/package.json
    {
      name: "pkg-1",
      version: "1.0.0",
      dependencies: {
        "pkg-2": "github:example-user/pkg-2#v1.0.0"
      }
    }

    // packages/pkg-2/package.json
    {
      name: "pkg-2",
      version: "1.0.0"
    }

在上面的例子中,

-   `lerna bootstrap`将正确的符号链接`pkg-2`链接到`pkg-1`. 
-   `lerna publish`将更新`pkg-1`的委托 (`#v1.0.0`) ,当`pkg-2`变化时. 

### readme徽章

用Lerna? 添加README徽章以显示它: [![lerna](https://img.shields.io/badge/maintained%20with-lerna-cc00ff.svg)](https://lernajs.io/)

    [![lerna](https://img.shields.io/badge/maintained%20with-lerna-cc00ff.svg)](https://lernajs.io/)

### 巫法

如果您更想要`cli`的一些指导 (如果您即将开始使用lerna或将其介绍给新团队) ,您可能会喜欢[lerna-wizard](https://github.com/szarouski/lerna-wizard). 它将引导您完成一系列明确定义的步骤: 

![lerna-wizard demo image](https://raw.githubusercontent.com/szarouski/lerna-wizard/2e269fb5a3af7100397a1f874cea3fa78089486e/demo.png)
