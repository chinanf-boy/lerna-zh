
# `@lerna/publish`

> 在当前项目中发布包

## 用法

```sh
lerna publish          # publish packages that have changed since the last release
lerna publish from-git # explicitly publish packages tagged in current commit
```

运行时,该命令执行下列操作之一: 

-   发布自上次发布以来更新的包 (调用) [`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#readme)幕后) . 
    -   这是勒纳2 X的遗留行为. 
-   发布在当前提交中标记的包 (`from-git`) 
-   发布一个未版本的"金丝雀"版本的包 (及其依赖者) 在以前的提交中更新. 

> LelNA将永远不会公布被标记为私有的包 (`"private": true`在`package.json`) 

**注: **若要发布作用域包,需要将下列内容添加到每个`package.json`: 

```json
  "publishConfig": {
    "access": "public"
  }
```

## 定位器

### 碰撞`from-git`

除了支持的SEMVER关键字[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#positionals),`lerna publish`也支持`from-git`关键字. 这将标识由`lerna version`并将它们发布到NPM. 这在您希望手动增加版本,但包内容本身由自动化流程一致发布的CI场景中很有用. 

## 选项

`lerna publish`支持所有提供的选项[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#options)除下列事项外: 

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
# 1.0.0 => 1.0.1-alpha.0+${SHA} of packages changed since the previous commit
# a subsequent canary publish will yield 1.0.1-alpha.1+${SHA}, etc

lerna publish --canary --preid beta
# 1.0.0 => 1.0.1-beta.0+${SHA}

# The following are equivalent:
lerna publish --canary minor
lerna publish --canary preminor
# 1.0.0 => 1.1.0-alpha.0+${SHA}
```

当使用此标志运行时,`lerna publish`以更细粒度的方式发布包 (每个提交) . 在发布到NPM之前,它会创建新的`version`取电流标签`version`把它撞到下一个*少数的*版本,添加所提供的元后缀 (默认为`alpha`) 并追加当前Git SHA (EX:`1.0.0`变成`1.1.0-alpha.81e3b443`) 

> 此标志的预期使用情况是每个提交级别的发布或夜间发布. 

### `--npm-client <client>`

必须是一个知道如何将包发布到NPM注册表的可执行文件. 默认值`--npm-client`是`npm`.

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

当使用此标志运行时,`lerna publish`将发布到NPM与给定的NPM[DIST标签](https://docs.npmjs.com/cli/dist-tag) (默认为`latest`) 

此选项可用于发布[`prerelease`](http://carrot.is/coding/npm_prerelease)或`beta`非下版本`latest`DIST标签,帮助消费者避免自动升级到预编码质量代码. 

> 注: `latest`标签是用户运行时使用的标签. `npm install my-package`. 若要安装不同的标签,用户可以运行`npm install my-package@prerelease`.

### `--no-verify-access`

默认情况下,`lerna`将验证登录的NPM用户对即将发布的包的访问. 通过此标志将禁用该检查. 

> 请谨慎使用

### `--no-verify-registry`

默认情况下,`lerna`将验证NPM注册表是否可访问,并验证当前NPM用户. 通过此标志将禁用该检查. 

> 请谨慎使用

### `--registry <url>`

当使用此标志运行时,转发的NPM命令将为您的包使用指定的注册表. 

如果不希望在所有package.json文件中单独显式地设置注册表配置 (例如,使用私有注册表) ,则这非常有用. 

### `--temp-tag`

传递时,此标志将首先将所有更改的包发布到临时磁盘标记 (`lerna-temp`然后将新版本移到默认版本[DIST标签](https://docs.npmjs.com/cli/dist-tag) (`latest`) 

这通常是不必要的,因为默认情况下,Lerna将以拓扑顺序 (依赖项之前的所有依赖项) 发布包. 

### `--yes`

```sh
lerna publish --canary --yes
# skips `Are you sure you want to publish the above changes?`
```

当使用此标志运行时,`lerna publish`将跳过所有确认提示. 有用的[连续集成 (CI) ](https://en.wikipedia.org/wiki/Continuous_integration)自动应答发布确认提示. 

## 弃权期权

### `--skip-npm`

呼叫[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#readme)直接代替. 
