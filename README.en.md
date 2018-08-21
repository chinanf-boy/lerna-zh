
<p align="center">
  <img alt="Lerna" src="https://cloud.githubusercontent.com/assets/952783/15271604/6da94f96-1a06-11e6-8b04-dc3171f79a90.png" width="480">
</p>

<p align="center">
  A tool for managing JavaScript projects with multiple packages.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/lerna"><img alt="NPM Status" src="https://img.shields.io/npm/v/lerna.svg?style=flat"></a>
  <a href="https://travis-ci.org/lerna/lerna"><img alt="Travis Status" src="https://img.shields.io/travis/lerna/lerna/master.svg?style=flat&label=travis"></a>
  <a href="https://ci.appveyor.com/project/lerna/lerna/branch/master"><img alt="Appveyor Status" src="https://img.shields.io/appveyor/ci/lerna/lerna/master.svg"></a>
  <a href="https://slack.lernajs.io/"><img alt="Slack Status" src="https://slack.lernajs.io/badge.svg"></a>
</p>

-   [关于](#about)
-   [入门](#getting-started)
-   [怎么运行的](#how-it-works)
-   [故障排除](#troubleshooting)
-   命令
    -   [`lerna publish`](./commands/publish#readme)
    -   [`lerna version`](./commands/version#readme)
    -   [`lerna bootstrap`](./commands/bootstrap#readme)
    -   [`lerna list`](./commands/list#readme)
    -   [`lerna changed`](./commands/changed#readme)
    -   [`lerna diff`](./commands/diff#readme)
    -   [`lerna exec`](./commands/exec#readme)
    -   [`lerna run`](./commands/run#readme)
    -   [`lerna init`](./commands/init#readme)
    -   [`lerna add`](./commands/add#readme)
    -   [`lerna clean`](./commands/clean#readme)
    -   [`lerna import`](./commands/import#readme)
    -   [`lerna link`](./commands/link#readme)
-   [概念](#concepts)
-   [Lerna.json](#lernajson)
-   [全球旗帜](./core/global-options)
-   [过滤标志](./core/filter-options)

## 关于

将大型代码库拆分为单独的独立版本包对于代码共享非常有用. 但是,在许多存储库中进行更改是*乱*并且难以跟踪,并且跨存储库的测试变得非常复杂. 

为了解决这些 (以及许多其他) 问题,一些项目会将其代码库组织到多包软件包中 (有时称为[monorepos](https://github.com/babel/babel/blob/master/doc/design/monorepo.md)) . 项目如[巴别塔](https://github.com/babel/babel/tree/master/packages),[应对](https://github.com/facebook/react/tree/master/packages),[角](https://github.com/angular/angular/tree/master/modules),[余烬](https://github.com/emberjs/ember.js/tree/master/packages),[流星](https://github.com/meteor/meteor/tree/devel/packages),[笑话](https://github.com/facebook/jest/tree/master/packages),以及许多其他人在一个存储库中开发所有包. 

**Lerna是一个优化使用git和npm管理多包存储库的工作流程的工具. **

Lerna还可以减少开发和构建环境中大量软件包副本的时间和空间需求 - 通常是将项目划分为多个单独的NPM软件包的缺点. 见[提升文件](doc/hoist.md)详情. 

### Lerna回购的样子是什么样的?

实际上它很少. 您有一个如下所示的文件系统: 

    my-lerna-repo/
      package.json
      packages/
        package-1/
          package.json
        package-2/
          package.json

### Lerna能做什么?

Lerna的两个主要命令是`lerna bootstrap`和`lerna publish`. 

`bootstrap`将把repo中的依赖关系链接在一起. `publish`将帮助发布任何更新的包. 

## 入门

> 以下说明适用于Lerna 3.x.对于新的Lerna项目,我们建议使用它而不是2.x.

让我们首先安装Lerna作为项目的dev依赖项[npm](https://www.npmjs.com/). 

```sh
$ mkdir lerna-repo && cd $_
$ npx lerna init
```

这将创建一个`lerna.json`配置文件以及`packages`文件夹,所以你的文件夹现在应该是这样的: 

    lerna-repo/
      packages/
      package.json
      lerna.json

## 怎么运行的

Lerna允许您使用以下两种模式之一管理项目: 固定或独立. 

### 固定/锁定模式 (默认) 

固定模式Lerna项目在单一版本线上运行. 版本保存在`lerna.json`文件位于项目的根目录下`version`键. 当你跑步`lerna publish`,如果自上次发布版本以来模块已更新,它将更新为您要发布的新版本. 这意味着您只需在需要时发布新版本的软件包. 

这是模式[巴别塔](https://github.com/babel/babel)目前正在使用. 如果要自动将所有包版本绑定在一起,请使用此选项. 这种方法的一个问题是任何包中的重大更改都将导致所有包具有新的主要版本. 

### 独立模式 (`--independent`) 

独立模式Lerna项目允许维护人员相互独立地增加包版本. 每次发布时,您都会收到每个已更改的包的提示,以指定它是补丁,次要,主要还是自定义更改. 

独立模式允许您更具体地更新每个包的版本,并对一组组件有意义. 将此模式与类似的结合起来[语义释放](https://github.com/semantic-release/semantic-release)会减少痛苦.  (目前已有相关工作[Atlassian的/勒拿湖语意释放](https://github.com/atlassian/lerna-semantic-release)) . 

> 该`version`键入`lerna.json`在独立模式下被忽略. 

## 故障排除

如果您在使用Lerna时遇到任何问题,请查看我们的[故障排除](doc/troubleshooting.md)记录您可能找到问题答案的文档. 

## 经常问的问题

看到[FAQ.md](FAQ.md). 

## 概念

Lerna将登录到`lerna-debug.log`文件 (同`npm-debug.log`) 当遇到运行命令的错误时. 

Lerna也有支持[范围包](https://docs.npmjs.com/misc/scope). 

运行`lerna`没有参数将显示所有命令/选项. 

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
-   `command.publish.ignoreChanges`: 一系列不包括在内的球体`lerna changed/publish`. 使用此选项可防止不必要地发布新版本以进行更改,例如修复`README.md`错字. 
-   `command.bootstrap.ignore`: 一系列在运行时不会被引导的globs`lerna bootstrap`命令. 
-   `command.bootstrap.npmClientArgs`: 将作为参数直接传递给的字符串数组`npm install`在此期间`lerna bootstrap`命令. 
-   `command.bootstrap.scope`: 一个globs数组,限制运行时将引导哪些包`lerna bootstrap`命令. 
-   `packages`: 用作包位置的globs数组. 

lerna.json中的包配置是一个匹配包含package.json的目录的globs列表,这是lerna如何识别"leaf"包.  (vs"root"package.json,用于管理整个repo的dev依赖项和脚本) . 

默认情况下,lerna将包列表初始化为`["packages/*"]`,但你也可以使用另一个目录,如`["modules/*"]`, 要么`["package1", "package2"]`. 定义的globs是相对于lerna.json所在的目录,通常是存储库根目录. 唯一的限制是您不能直接嵌套包位置,但这也是"普通"npm包共享的限制. 

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

找到叶包下面`packages/*`被认为是"最佳实践",但不是使用Lerna的要求. 

### 共同`devDependencies`

最`devDependencies`可以拉到Lerna回购的根源. 

这有一些好处: 

-   所有包都使用给定依赖项的相同版本
-   使用自动化工具可以使根目录的依赖关系保持最新状态[GreenKeeper](https://greenkeeper.io/)
-   依赖安装时间减少
-   需要更少的存储空间

注意`devDependencies`提供npm脚本使用的"二进制"可执行文件仍然需要直接安装在每个使用它们的包中. 

比如说`nsp`在这种情况下,依赖是必要的`lerna run nsp` (和`npm run nsp`在包的目录内) 才能正常工作: 

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

### Git Hosted Dependencies

Lerna允许将本地依赖包的目标版本编写为[git remote url](https://docs.npmjs.com/cli/install)用一个`committish` (例如. ,`#v1.0.0`要么`#semver:^1.0.0`) 而不是正常的数字版本范围. 当包必须是私有的时,这允许通过git存储库分发包[私人npm注册表是不可取的](https://www.dotconferences.com/2016/05/fabien-potencier-monolithic-repositories-vs-many-repositories). 

请注意lerna*不*执行实际将git历史记录拆分到单独的只读存储库中. 这是用户的责任.  (看到[这个评论](https://github.com/lerna/lerna/pull/1033#issuecomment-335894690)实施细节) 

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

-   `lerna bootstrap`将正确符号链接`pkg-2`成`pkg-1`. 
-   `lerna publish`将更新委托 (`#v1.0.0`) `pkg-1`什么时候`pkg-2`变化. 

### 自述文件徽章

用Lerna?添加README徽章以显示它: [![lerna](https://img.shields.io/badge/maintained%20with-lerna-cc00ff.svg)](https://lernajs.io/)

    [![lerna](https://img.shields.io/badge/maintained%20with-lerna-cc00ff.svg)](https://lernajs.io/)

### 巫师

如果您更喜欢cli的一些指导 (如果您即将开始使用lerna或将其介绍给新团队) ,您可能会喜欢[勒拿湖的向导](https://github.com/szarouski/lerna-wizard). 它将引导您完成一系列明确定义的步骤: 

![lerna-wizard demo image](https://raw.githubusercontent.com/szarouski/lerna-wizard/2e269fb5a3af7100397a1f874cea3fa78089486e/demo.png)
