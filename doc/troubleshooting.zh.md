
# 故障排除

本文档包含针对用户过去在使用Lerna时遇到的某些问题的解决方案. 

## 引导命令

### 使用纱线作为NPM客户端时的错误

在释放勒纳之前`v2.0.0-rc.3`国旗的使用者`--npm-client`作为客户机提供纱线的人可能遭受了引导过程不能正常运行的痛苦. 

    Error running command.
    error Command failed with exit code 1.

如果您可以将LeRNA升级到所述版本,请这样做,或者作为替代方案,您可以添加`--concurrency=1`.

### 私有NPM注册表 (艺术工厂ㄡNPM企业等) 集成问题

如果`lerna bootstrap`失败,因为私有服务器上有存储库,请确保包含该注册表. 例子: `lerna bootstrap -- --registry=http://[registry-url]`.

## 导入命令

### 进口过程中的缓冲问题

当您试图导入一个在其中有许多提交的存储库时,可能会出现错误,例如: 

    DeprecationWarning: Unhandled promise rejections are deprecated

或

    Error: spawnSync /bin/sh ENOBUFS during ImportCommand.execute

#### 解决方案: 

跑`lerna import`与`--max-buffer`标志,并提供足够大的数字 (以字节为单位) . 在写这个条目时,默认的默认值是10MB,所以你应该记住这一点. 

### 合并冲突提交不能导入

当试图导入包含需要冲突解决的合并提交的存储库时,导入命令失败,并出错: 

    lerna ERR! execute Error: Command failed: git am -3
    lerna ERR! execute error: Failed to merge in the changes.
    lerna ERR! execute CONFLICT (content): Merge conflict in [file]

#### 解决方案

跑`lerna import`与`--flatten`用于在"平面"模式下导入历史记录的标志,即,将每次合并提交作为合并引入的单个更改. 

### Git树未提交更改时失败

你会得到`fatal: ambiguous argument 'HEAD':`错误,当当前项目有**未提交更改**.

#### 解决方案

在导入任何包 (使用`lerna import`.

## 发布命令

### 发布不使用GITHUB/GITHUB企业以固定模式检测手动创建的标签

GITHUB和GITHUB企业在通过以下内容创建发布时使用轻量级Git标签[网络用户界面](https://help.github.com/articles/working-with-tags)而LeRNA则使用带注释的标签. 

这可能导致Lerna忽略以前发布的版本 (这些版本已经被手动执行并用Github web ui标记) 的问题. 

例如,发布历史如下: 

-   V1.1.0发布并标注`lerna publish`
-   V1.2.0被手动发布并用GITHUB WebUI标记
-   V1.2.1被手动发布并用GITHUB WebUI标记

运行`lerna publish`现在将检测V1.1.0,而不是V1.2.1作为最后发布的标签. 

这取决于你的使用情况. `lerna publish`: 

-   发布提示将使用V1.1.0作为大调/小调/补丁建议的基础. 
-   当使用-----常规提交标志: 
    -   根据V1.1.0 (包括V1.2.0ㄡV1.2.1等的提交) ,建议基于所有提交的SEVER增量. 
    -   生成的CeleLog.MD文件将重复已经在V1.2.0ㄡV1.2.1等中发布的所有提交. 

#### 解决方案: 

如果可能的话,使用`lerna publish`手动释放. 

对于新的手动释放,使用`git tag -a -m <version>`而不是使用GITHUB WebUI. 

对于现有的轻量级标签,可以使用类似的方法将它们转换为带注释的标签: 

```sh
GIT_AUTHOR_NAME="$(git show $1 --format=%aN -s)"
GIT_AUTHOR_EMAIL="$(git show $1 --format=%aE -s)"
GIT_AUTHOR_DATE="$(git show $1 --format=%aD -s)"
GIT_COMMITTER_NAME="$(git show $1 --format=%cN -s)"
GIT_COMMITTER_EMAIL="$(git show $1 --format=%cE -s)"
GIT_COMMITTER_DATE="$(git show $1 --format=%cD -s)"

git tag -a -m $1 -f $1 $1

git push --tags --force
```

看到这个[栈溢出柱](https://stackoverflow.com/questions/5002555/can-a-lightweight-tag-be-converted-to-an-annotated-tag)更多细节

### 发布到一个私人的NPM注册表 (艺人,NPM企业等) 

如果`lerna publish`是失败,确保你有以下你的`package.json`: 

```javascript
	"publishConfig": {
		"registry": "https://[registry-url]"
	}
```

您可能还需要向您添加以下内容`.npmrc`个人包上的文件: 

    registry = https://[registry-url]
