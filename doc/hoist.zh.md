
# 勒纳吊装

> 启用此功能时要谨慎,因为某些配置可能会导致问题. 

当一个整体项目被划分为多个NPM包时,这种组织上的改进通常伴随着成本: 不同的包在其中通常具有许多重复的依赖性`package.json`文件,因此数以百计或数千个重复的文件在各种`node_modules`目录. 通过简化对由许多NPM包组成的项目的管理,Lerna可能会在不经意间加剧这个问题. 

幸运的是,Lerna还提供了一个特性来改善这种情况ℴℴ通过将共享依赖关系提升到Lerna项目级别的最顶层,Lerna可以减少开发和构建环境中大量软件包副本的时间和空间需求`node_modules`目录代替. 

`--hoist`其目的在于使用透明,理想情况下不需要对项目进行任何其他修改的运行时优化. 当`--hoist`使用标志: 

-   将安装公共依赖项*只有*到顶层`node_modules`个别包装省略`node_modules`.
-   大多数常见的依赖关系仍然被提升,但是具有不同版本的孤立包将获得一个正常的本地包`node_modules`安装必要的依赖项. 
    -   在这种情况下,`lerna bootstrap`将永远使用`npm install`与`--global-style`标志,不管客户端配置. 
-   这些普通包的二进制文件与单个包相连接. `node_modules/.bin`目录,以便`package.json`脚本继续进行未修改的工作. 
-   性能良好的基于节点的软件应该继续工作. 

## 模块分辨率

这个[节点模块分解算法](https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders)是递归的: 当寻找包时`A`它看起来像是本地的`node_modules/A`目录,然后`../node_modules/A`,`../../node_modules/A`,`../../../node_modules/A`等. 

遵循此规范的工具可以透明地找到已经被提升的依赖关系. 

不幸的是,一些工具并不严格遵循模块解析规范,而是假设或要求依赖项在本地特定地存在. `node_modules`目录. 为了解决这一问题,有可能将包裹从其升迁的顶级位置与单个包装进行链接. `node_modules`目录. Lerna还没有自动完成此操作,建议与工具维护人员一起工作以迁移到更兼容的模式. 
