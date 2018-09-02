
# Lerna Hoist

> 启用此功能时要谨慎,因为某些配置可能会导致问题. 

当一个整体项目被划分为多个NPM包时,这种组织上的改进通常伴随着成本: 在其中，不同的包的`package.json`文件通常具有许多重复的依赖性,因此在各种`node_modules`目录存在数以百计或数千个重复的文件. 通过简化对由许多NPM包组成的项目的管理,Lerna可能会在不经意间加剧这个问题. 

幸运的是,Lerna还提供了一个特性来改善这种情况 - 通过将共享依赖关系提升到Lerna项目级别, 最顶层的`node_modules`目录.这样,在开发和构建环境中,Lerna可以减少大量软件包副本的时间和空间需求. 

`--hoist`其目的在于使用透明,理想情况下不需要对项目进行任何其他修改,正常运行时就可以做到优化. 当使用`--hoist`标志: 

-   *只*将公共依赖项安装到顶层的`node_modules`,而个别包装省略`node_modules`.
-   大多数常见的依赖项仍然被提升,但是具有不同版本的孤立包,将获得一个正常的本地`node_modules`包,其安装必要的依赖项. 
    -  在这种情况下,`lerna bootstrap`将永远使用`npm install`带`--global-style`标志,而不去管客户端配置. 
-   这些常用包的二进制文件与单个包中`node_modules/.bin`目录对应连接. ,以便`package.json`脚本能继续进行未修改的工作. 
-   性能良好的基于Node的软件,应该继续工作. 

## 模块分辨率

这个[Node模块分解算法](https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders)是递归的: 当寻找`A`包时,它会在本地的`node_modules/A`目录中查找,然后`../node_modules/A`,`../../node_modules/A`,`../../../node_modules/A`等. 

遵循此规范的工具,可以透明地,且顺畅地找到已经被提升的依赖项. 

不幸的是,一些工具并不严格遵循模块解析规范,而是假设或要求依赖项在本地特定`node_modules`目录存在. 为了解决这一问题,有可能将 包从其提升的顶级位置,与单个包的`node_modules`目录进行链接. Lerna还没有自动完成此操作,建议改为与工具维护人员合作
,以迁移到更兼容的模式。
