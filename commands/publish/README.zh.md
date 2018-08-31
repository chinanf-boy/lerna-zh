
# `@lerna/publish`

> 在当前项目中发布包

## 用法

```sh
lerna publish          # 发布更新的包, 自上次发布版本
lerna publish from-git # 显式发布当前提交中标记的包
```

运行时,该命令执行下列操作之一: 

-   发布自上次发布以来更新的包 (幕后调用 [`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#readme)) . 
    -   这是 lerna2.X 的遗留行为. 
-   发布在当前提交中标记的包 (`from-git`) 
-   发布在之前的提交中,更新的未经版本化的“canary”版本的软件包(及其依赖项).

> Lerna将永远不会公布,被标记为私有的包 (`"private": true`在`package.json`) 

**注:**若要发布作用域包,需要将下列内容添加到每个`package.json`: 

```json
  "publishConfig": {
    "access": "public"
  }
```

## 定位器

### 碰撞`from-git`

除了[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#positionals)支持semver关键字,`lerna publish`也支持`from-git`关键字. 这将由`lerna version`标识,并将它们发布到npm. 这在您希望手动增加版本的CI场景中很有用，
但是包装内容本身是由自动化过程一致发布的。

## 选项

`lerna publish`支持[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#options)所有提供的选项,除下列事项外: 

-   [`--canary`](#--canary)
-   [`--npm-client <client>`](#--npm-client-client)
-   [`--npm-tag <dist-tag>`](#--npm-tag-dist-tag)
-   [`--no-verify-access`](#--no-verify-access)
-   [`--no-verify-registry`](#--no-verify-registry)
-   [`--registry <url>`](#--registry-url)
-   [`--temp-tag`](#--temp-tag)
-   [`--yes`](#--yes)

### `--canary`

```sh
lerna publish --canary
# 1.0.0 => 1.0.1-alpha.0+${SHA} 包更改, 自上次提交
# 随后的金丝雀出版将会产生 1.0.1-alpha.1+${SHA}, etc

lerna publish --canary --preid beta
# 1.0.0 => 1.0.1-beta.0+${SHA}

# The following are equivalent:
lerna publish --canary minor
lerna publish --canary preminor
# 1.0.0 => 1.1.0-alpha.0+${SHA}
```

当使用此参数运行时,`lerna publish`以更细粒度的方式发布包 (每个提交) . 在发布到npm之前,它会创建新的`version`,取当前`version`把它撞到下一个*minor*版本,添加所提供的元后缀 (默认为`alpha`) 并追加当前Git SHA (EX:`1.0.0`变成`1.1.0-alpha.81e3b443`) 

> 此参数的预期使用情况是 一个预提交的发布或 nightly发布. 

### `--npm-client <client>`

必须是一个知道如何,将包发布到npm注册表的可执行文件. 默认值`--npm-client`是`npm`.

```sh
lerna publish --npm-client yarn
```

也可以配置为`lerna.json`: 

```json
{
  "command": {
    "publish": {
      "npmClient": "yarn"
    }
  }
}
```

### `--npm-tag <dist-tag>`

```sh
lerna publish --npm-tag next
```

当使用此参数运行时,`lerna publish`将发布到npm与给定的npm[dist tag](https://docs.npmjs.com/cli/dist-tag) (默认为`latest`) 

此选项可用于发布[`prerelease`](http://carrot.is/coding/npm_prerelease)或`beta`版本,非下个`latest`标签版本,来帮助消费者避免自动升级到预编码质量代码. 

> 注: `latest`标签是用户运行时使用的标签. `npm install my-package`. 若要安装不同的标签,用户可以运行`npm install my-package@prerelease`.

### `--no-verify-access`

默认情况下,`lerna`将验证登录的npm用户对即将发布的包的访问. 通过此参数将禁用该检查. 

> 请谨慎使用

### `--no-verify-registry`

默认情况下,`lerna`将验证npm注册表是否可访问,并验证当前npm用户. 通过此参数将禁用该检查. 

> 请谨慎使用

### `--registry <url>`

当使用此参数运行时,接下来的npm命令将为您的包使用指定的注册表. 

如果您不想在所有package.json文件中单独显式设置注册表配置，这将非常有用
，例如使用私人注册表.

### `--temp-tag`

传递时,此参数首先将所有更改的包 发布 到 临时磁盘标记 (`lerna-temp`)然后将新版本移到默认版本[dist tag](https://docs.npmjs.com/cli/dist-tag) (`latest`) 

这通常是不必要的,因为默认情况下,Lerna将以顺序拓扑 (依赖项之前的所有依赖项) 发布包. 

### `--yes`

```sh
lerna publish --canary --yes
# 跳过 `Are you sure you want to publish the above changes?`
```

当使用此参数运行时,`lerna publish`将跳过所有确认提示. 有用的[自动化集成 (CI) ](https://en.wikipedia.org/wiki/Continuous_integration)自动应答发布确认提示. 

## 不推荐的选项

### `--skip-npm`

直接代替运行[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#readme). 
