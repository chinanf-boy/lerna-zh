# `@lerna/version`

> 自上次发布以来，撞出包更改的版本

## Usage

```sh
lerna version 1.0.1 # 明确的
lerna version patch # semver 关键字
lerna version       # 提示选择项(s)
```

运行时,做以下事情:

1.  自上个标注版本,明确更改包的版本.
2.  提示新版本.
3.  重新调整元数据以反映新版本.
4.  commit 更改与添加标签 commit.
5.  推送 git 远程.

## 版本定位

### `撞出`semver

```sh
lerna version [major | minor | patch | premajor | preminor | prepatch | prerelease]
# 使用 下一代语义版本 (s) 变量 和 跳过`选择新版本`的提示
```

当带有这些选项运行时,`lerna version`将跳过版本选择提示 和 [提升](https://github.com/npm/node-semver#functions)对应语义的版本,你可以使用`--yes`避免所有的选项的提示.

#### 预发布"毕业了"

如果你有任何带有预发行版本号的软件包（例如`2.0.0-beta.3`）并运行`lerna version` + 没有预发布版本（如`major`，`minor`或`patch`） ，它将发布那些以前预发布的包 _以及_ 自上次发布以来已更改的包。

## 选项

- [`--allow-branch`](#--allow-branch-glob)
- [`--amend`](#--amend)
- [`--commit-hooks`](#--commit-hooks)
- [`--conventional-commits`](#--conventional-commits)
- [`--changelog-preset`](#--changelog-preset)
- [`--exact`](#--exact)
- [`--force-publish`](#--force-publish)
- [`--ignore-changes`](#--ignore-changes)
- [`--git-remote`](#--git-remote-name)
- [`--git-tag-version`](#--git-tag-version)
- [`--message`](#--message-msg)
- [`--preid`](#--preid)
- [`--push`](#--push)
- [`--sign-git-commit`](#--sign-git-commit)
- [`--sign-git-tag`](#--sign-git-tag)

### `--allow-branch <glob>`

当启用了“lerna version”时,匹配 git 分支 的 glob 模式白名单.
在`lerna.json`中配置是最简单的（也是推荐的），但也可以作为 CLI 选项传递。

```json
{
  "command": {
    "publish": {
      "allowBranch": "master"
    }
  }
}
```

通过上面的配置,`lerna version`在其他分支运行时失败,只在`master`成功. 这个选项被认为是限制`lerna version`单独主分支的最佳实践.

```json
{
  "command": {
    "publish": {
      "allowBranch": ["master", "feature/*"]
    }
  }
}
```

在上面的配置中,`lerna version`允许在前缀`feature/`的任何分支运行. 请注意,当分支 合并到 主分支 中 时,你在功能分支中生成的 git 标记充满了潜在的错误. 如果标记与主线的上下文"分离"开来 (可能是通过 压缩合并 或 冲突合并 提交的) ,将来`lerna version`执行将难以确定正确的"自上次发布以来的差异".

总是可以在命令行上,覆盖这个"耐用"配置. 请谨慎使用.

```sh
lerna version --allow-branch hotfix/oops-fix-the-thing
```

### `--amend`

```sh
lerna version --amend
# commit message is retained, and `git push` is skipped.
```

当使用此选项运行时,`lerna version`将组合当前提交的所有更改,而不是添加新的更改. 这是有用的. [连续集成 (CI) ](https://en.wikipedia.org/wiki/Continuous_integration)时就可以减少项目历史中提交的数量.

为了防止意外的重写,此命令将跳过`git push` (也就是说,它意味着`--no-push`)

### `--commit-hooks`

在提交版本更改时,运行 Git 提交挂钩.

默认为`true`. 传递`--no-commit-hooks`禁用.

这个选项类似于`npm version` 同名的[选项](https://docs.npmjs.com/misc/config#commit-hooks)

### `--conventional-commits`

```sh
lerna version --conventional-commits
```

当使用此选项运行时,`lerna version`将使用[常规提交规范](https://conventionalcommits.org/)规定[确定版本](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-recommended-bump)和[生成变更日志](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli)

### `--changelog-preset`

```sh
lerna version --conventional-commits --changelog-preset angular-bitbucket
```

默认情况下,更改日志 预设置为[`angular`](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-angular#angular-convention). 在某些情况下,您可能希望使用另一个预设或自定义更改.

预设置可以是内置或可安装配置的传统的变更日志. 预设置可以作为包的全名传递,或者自动扩展后缀 (例如,`angular`扩展到`conventional-changelog-angular`)

### `--exact`

```sh
lerna version --exact
```

当使用此选项运行时,`lerna version`将在更新包中,准确地指定更新的依赖项 (没有标点符号) ,而不是作为 semver 兼容的 (`^`符号)

有关更多信息,请参见 package.json 的[依赖关系](https://docs.npmjs.com/files/package.json#dependencies)文档.

### `--force-publish`

```sh
lerna version --force-publish=package-2,package-4

# force all packages to be versioned
lerna version --force-publish
```

当使用此选项运行时,`lerna version`将强制发布指定的包 (逗号分隔) 或所有包使用`*`.

> 这将跳过`lerna changed`检查更改的包,并强制更新,即便一个包没有`git diff`更改.

### `--ignore-changes`

当检测到更改的包时,忽略 匹配了 glob 模式的文件 的更改.

```sh
lerna version --ignore-changes '**/*.md' '**/__tests__/**'
```

此选项最好设置在主`lerna.json`配置中,既避免 shell 过早测定 glob 的匹配,又能与`lerna diff`和`lerna changed`共享配置:

```json
{
  "ignoreChanges": ["**/__fixtures__/**", "**/__tests__/**", "**/*.md"]
}
```

传递`--no-ignore-changes`禁用任何现有的持久配置.

### `--git-remote <name>`

```sh
lerna version --git-remote upstream
```

当使用此选项运行时,`lerna version`将 Git 更改推送到指定的远程,而不是`origin`.

### `--git-tag-version`

提交,并标记版本的更改.

默认为`true`. 通过`--no-git-tag-version`禁用.

这个选项类似于`npm version` 同名的[参数选项](https://docs.npmjs.com/misc/config#git-tag-version).

### `--message <msg>`

此选项别名为`-m`,是与`git commit`一样的使用方式.

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

使用此选项运行时,`lerna version`在提交版本更新,将使用提供的消息. 将 lerna 集成到 期望提交消息 遵守某些准则 的项目中,这个选项会有用,就像多个项目使用[commitizen](https://github.com/commitizen/cz-cli)和/或[semantic-release](https://github.com/semantic-release/semantic-release).

如果消息包含`%s`,它将被替换为前缀为"v"的新全局版本号. 如果消息包含`%v`,它将被替换为没有前导"v"的新的全局版本号. 请注意,这仅适用于使用默认"固定"版本控制模式,因为独立版本控制时没有"全局"版本.

这可以在 lerna.json 中配置:

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

使用此选项运行时`lerna version`会增加`premajor`,`preminor`,`prepatch`, 要么`prerelease`的版本,通过 semver 指定的[预发布标识符](http://semver.org/#spec-item-9).

### `--push`

将已提交,和,已标记的更改,推送到已配置的[git remote](https://github.com/lerna/lerna/tree/master/commands/version#--git-remote)

### `--sign-git-commit`

这个选项类似于`npm version` 同名的[参数选项](https://docs.npmjs.com/misc/config#sign-git-commit).

### `--sign-git-tag`

这个选项类似于`npm version` 同名的[参数选项](https://docs.npmjs.com/misc/config#sign-git-tag).

## 不推荐的选项

### `--cd-version`

将 semver 关键字传递给[`bump`](#bump)取代版本定位.

### `--repo-version`

将明确的版本号传递给[`bump`](#bump)取代版本定位.

### `--skip-git`

使用[`--no-git-tag-version`](https://github.com/lerna/lerna/tree/master/commands/version#--git-tag-version)和[`--no-push`](https://github.com/lerna/lerna/tree/master/commands/version#--push)代替.

> 注意: 此选项**才不是**限制*所有*git 命令被执行. `git`仍然需要`lerna version`.
