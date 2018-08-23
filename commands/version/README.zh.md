
# `@lerna/version`

> ㄰中文解释ㄱ: 布麦语版

## Usage

```sh
lerna version 1.0.1 # explicit
lerna version patch # semver keyword
lerna version       # select from prompt(s)
```

When Run,this command does the following: 

1.  4 . Identfies Packages that have been updated since the previous tagged release .
2.  新版本. 
3.  Refkage Metadatta to reflect new release .
4.  这些变化commits and the标签提交. 
5.  pushes to the git远程. 

## Positionals

### semver`bump`

```sh
lerna version [major | minor | patch | premajor | preminor | prepatch | prerelease]
# uses the next semantic version(s) value and this skips `Select a new version for...` prompt
```

当run with this的旗帜,`lerna version`将跳过版本选择prompt and the[难以置信的](https://github.com/npm/node-semver#functions)The version by that Keyword .You must still use the`--yes`为了避免所有的旗帜的提示. 

#### "graduating prereleases"

如果你有任何的预发布版本的软件包 (如with a number`2.0.0-beta.3`你跑) `lerna version`with and抢鲜撞击 (非`major`,`minor`金,`patch`它将那些以前出版) ,预发布的软件包*as well as*The Packages that have hanged since the last release .

## 选项

-   [`--allow-branch`](#--allow-branch-glob)
-   [`--amend`](#--amend)
-   [`--commit-hooks`](#--commit-hooks)
-   [`--conventional-commits`](#--conventional-commits)
-   [`--changelog-preset`](#--changelog-preset)
-   [`--exact`](#--exact)
-   [`--force-publish`](#--force-publish)
-   [`--ignore-changes`](#--ignore-changes)
-   [`--git-remote`](#--git-remote-name)
-   [`--git-tag-version`](#--git-tag-version)
-   [`--message`](#--message-msg)
-   [`--preid`](#--preid)
-   [`--push`](#--push)
-   [`--sign-git-commit`](#--sign-git-commit)
-   [`--sign-git-tag`](#--sign-git-tag)

### `--allow-branch <glob>`

如果您发现有错误,请尽管发表评论!`lerna version`是的. easiest (It is recommended to configure and in) `lerna.json`,but it is possible to pass as well as a命令行选项. 

```json
{
  "command": {
    "publish": {
      "allowBranch": "master"
    }
  }
}
```

通过上面的配置,`lerna version`将从其他分支运行时失败`master`. 它被认为是限制的最佳实践. `lerna version`一枝独行. 

```json
{
  "command": {
    "publish": {
      "allowBranch": ["master", "feature/*"]
    }
  }
}
```

在前面的配置中,`lerna version`将允许在任何分支前缀`feature/`. 请注意,当分支合并到主分支中时,在特征分支中生成git标记充满了潜在的错误. 如果标记与原始上下文"分离" (可能通过压缩合并或冲突合并提交) ,将来`lerna version`执行将难以确定正确的"自上次发布以来的差异". 

总是可以在命令行上重写这个"耐用"配置. 请谨慎使用. 

```sh
lerna version --allow-branch hotfix/oops-fix-the-thing
```

### `--amend`

```sh
lerna version --amend
# commit message is retained, and `git push` is skipped.
```

当使用此标志运行时,`lerna version`将执行当前提交的所有更改,而不是添加新的更改. 这是有用的. [连续集成 (CI) ](https://en.wikipedia.org/wiki/Continuous_integration)以减少项目历史中提交的数量. 

为了防止意外的重写,此命令将跳过. `git push` (也就是说,它意味着`--no-push`) 

### `--commit-hooks`

在提交版本更改时运行Git提交挂钩. 

默认为`true`. 孔型`--no-commit-hooks`禁用. 

这个选项类似于`npm version` [选项](https://docs.npmjs.com/misc/config#commit-hooks)同名的

### `--conventional-commits`

```sh
lerna version --conventional-commits
```

当使用此标志运行时,`lerna version`将使用[常规提交规范](https://conventionalcommits.org/)到[确定版本颠簸](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-recommended-bump)和[生成变更日志](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli)

### `--changelog-preset`

```sh
lerna version --conventional-commits --changelog-preset angular-bitbucket
```

默认情况下,更改LeelOG预置设置为[`angular`](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-angular#angular-convention). 在某些情况下,您可能希望使用另一个预设或自定义更改. 

预设是传统的变更文件的内置或可安装配置的名称. 预设可以作为包的全名传递,或者自动扩展后缀 (例如,`angular`扩展到`conventional-changelog-angular`) 

### `--exact`

```sh
lerna version --exact
```

当使用此标志运行时,`lerna version`将在更新的包中准确地指定更新的依赖项 (没有标点符号) ,而不是作为semver兼容的 (与`^`) 

有关更多信息,请参见PACGAG.JSON[依赖关系](https://docs.npmjs.com/files/package.json#dependencies)文档. 

### `--force-publish`

```sh
lerna version --force-publish=package-2,package-4

# force all packages to be versioned
lerna version --force-publish
```

当使用此标志运行时,`lerna version`将强制发布指定的包 (逗号分隔) 或所有包使用`*`.

> 这将跳过`lerna changed`检查更改的包并强制没有一个包`git diff`更改要更新. 

### `--ignore-changes`

当检测到更改的包时,忽略由GLUB匹配的文件中的更改. 

```sh
lerna version --ignore-changes '**/*.md' '**/__tests__/**'
```

此选项最好指定为root. `lerna.json`配置,既避免过早对shell进行shell评估,又与共享共享配置. `lerna diff`和`lerna changed`: 

```json
{
  "ignoreChanges": [
    "**/__fixtures__/**",
    "**/__tests__/**",
    "**/*.md"
  ]
}
```

孔型`--no-ignore-changes`禁用任何现有的持久配置. 

### `--git-remote <name>`

```sh
lerna version --git-remote upstream
```

当使用此标志运行时,`lerna version`将Git更改推送到指定的远程,而不是`origin`. 

### `--git-tag-version`

提交并标记版本化更改. 

默认为`true`. 通过`--no-git-tag-version`禁用. 

这个选项类似于`npm version` [选项](https://docs.npmjs.com/misc/config#git-tag-version)同名. 

### `--message <msg>`

此选项具有别名`-m`与...平价`git commit`. 

```sh
lerna version -m "chore(release): publish %s"
# commit message = "chore(release): publish v1.0.0"

lerna version -m "chore(release): publish %v"
# commit message = "chore(release): publish 1.0.0"

# When versioning packages independently, no placeholders are replaced
lerna version -m "chore(release): publish"
# commit message = "chore(release): publish
#
# - package-1@3.0.1
# - package-2@1.5.4"
```

使用此标志运行时`lerna version`在提交版本更新以供发布时,将使用提供的消息. 用于将lerna集成到期望提交消息遵守某些准则的项目中,例如使用的项目[commitizen](https://github.com/commitizen/cz-cli)和/或[语义释放](https://github.com/semantic-release/semantic-release). 

如果消息包含`%s`,它将被替换为前缀为"v"的新全球版本号. 如果消息包含`%v`,它将被替换为没有前导"v"的新的全球版本号. 请注意,这仅适用于使用默认"固定"版本控制模式,因为独立版本控制时没有"全局"版本. 

这可以在lerna.json中配置: 

```json
{
  "command": {
    "publish": {
      "message": "chore(release): publish %s"
    }
  }
}
```

### `--preid`

```sh
lerna version prerelease
# uses the next semantic prerelease version, e.g.
# 1.0.0 => 1.0.1-alpha.0

lerna version prepatch --preid next
# uses the next semantic prerelease version with a specific prerelease identifier, e.g.
# 1.0.0 => 1.0.1-next.0
```

使用此标志运行时`lerna version`会增加`premajor`,`preminor`,`prepatch`, 要么`prerelease`使用指定的semver bumps[预发布标识符](http://semver.org/#spec-item-9). 

### `--push`

将已提交和已标记的更改推送到已配置的更改[git remote](https://github.com/lerna/lerna/tree/master/commands/version#--git-remote)

### `--sign-git-commit`

这个选项类似于`npm version` [选项](https://docs.npmjs.com/misc/config#sign-git-commit)同名. 

### `--sign-git-tag`

这个选项类似于`npm version` [选项](https://docs.npmjs.com/misc/config#sign-git-tag)同名. 

## 不推荐的选项

### `--cd-version`

将semver关键字传递给[`bump`](#bump)位置而不是. 

### `--repo-version`

将明确的版本号传递给[`bump`](#bump)位置而不是. 

### `--skip-git`

使用[`--no-git-tag-version`](https://github.com/lerna/lerna/tree/master/commands/version#--git-tag-version)和[`--no-push`](https://github.com/lerna/lerna/tree/master/commands/version#--push)代替. 

> 注意: 此选项**才不是**限制*所有*git命令被执行. `git`仍然需要`lerna version`. 
