
# 故障排除

本文档包含过去用户在使用Lerna时,遇到的某些问题的解决方案. 

## Bootstrap命令

### 使用 yarn 作为NPM客户端时的错误

在用户确定`v2.0.0-rc.3`版本之前,`--npm-client`参数使用 yarn 的人可能遭受了 bootstrap 过程不能正常运行的痛苦. 

    Error running command.
    error Command failed with exit code 1.

如果您可以将 Lerna 升级到所述版本,请这样做,或者作为替代方案,您可以添加`--concurrency=1`.

### 私有NPM注册表 (Artifactory, npm Enterprise 等) 集成问题

如果`lerna bootstrap`的失败,是因为私有服务器上有存储库,请确保包含该注册表. 例子: `lerna bootstrap -- --registry=http://[registry-url]`.

## 导入命令

### 导入过程中的缓冲问题

当您试图导入一个在其中有许多提交的存储库时,可能会出现错误,例如: 

    DeprecationWarning: Unhandled promise rejections are deprecated

或

    Error: spawnSync /bin/sh ENOBUFS during ImportCommand.execute

#### 解决方案: 

运行`lerna import`带有`--max-buffer`参数,并提供足够大的数字 (以字节为单位) . 在写这个条目时,默认的默认值是10MB,所以你应该记住这一点. 

### 合并冲突提交不能导入

当试图导入包含需要冲突解决的合并提交的存储库时,导入命令失败,并出错: 

    lerna ERR! execute Error: Command failed: git am -3
    lerna ERR! execute error: Failed to merge in the changes.
    lerna ERR! execute CONFLICT (content): Merge conflict in [file]

#### 解决方案

运行`lerna import`带有`--flatten`会在"flay"模式下导入历史记录的标志,即,将每次合并提交都作为单个合并引入的更改. 

### Git树未提交更改时失败

你会得到`fatal: ambiguous argument 'HEAD':`错误,当前项目有**未提交更改**.

#### 解决方案

确保在使用`lerna import`导入任何包之前, commit 你lerna项目的更改.

## 发布命令

### 发布不使用 github/github企业 以固定模式检测手动创建的标签

github和github企业 在创建发布时,是通过[网络用户界面](https://help.github.com/articles/working-with-tags)来使用轻量级Git标签,而Lerna则使用带注释的标签. 

这可能导致Lerna忽略以前发布的版本 (这些版本已经被手动执行并用Github web ui标记) 的问题. 

例如,发布历史如下: 

-   v1.1.0发布并标注通过`lerna publish`
-   v1.2.0被手动发布通过github WebUI标记
-   v1.2.1被手动发布通过github WebUI标记

现在,运行`lerna publish`将检测v1.1.0,而不是v1.2.1作为最后发布的标签. 

这取决于你的使用`lerna publish`情况: 

-   发布提示将使用 v1.1.0 作为 major/minor/patch 的基础. 
-   当使用-----常规提交标志: 
    -   建议基于v1.1.0所有提交的semver增量(包括v1.2.0,v1.2.1等的提交) . 
    -   生成的CHANGELOG.md文件将重复已经在 v1.2.0,v1.2.1 等中发布的所有提交. 

#### 解决方案: 

如果可能的话,全部使用`lerna publish`手动版本. 

对于新的手动版本,使用`git tag -a -m <version>`而不是使用github WebUI. 

对于现有的轻量级标签,可以使用类似的方法,将它们转换为带注释的标签: 

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

看到这个[stackoverflow](https://stackoverflow.com/questions/5002555/can-a-lightweight-tag-be-converted-to-an-annotated-tag)的更多细节

### 发布到一个私人的NPM注册表 (Artifactory,npm Enterprise等) 

如果`lerna publish`失败,确保你的`package.json`有: 

```javascript
	"publishConfig": {
		"registry": "https://[registry-url]"
	}
```

您可能还需要向您包中`.npmrc`文件添加以下内容: 

    registry = https://[registry-url]
