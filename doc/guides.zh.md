
# 指南

## JTest/VisualStudio代码调试

调试是可能的[开玩笑](https://facebook.github.io/jest/)使用LRNA管理包的测试[VisualStudio代码](https://code.visualstudio.com/). 使用断点调试与在MMOREPO的下面的VSCODE启动配置一起工作`<root>/.vscode/launch.json`文件. 这个例子为单个软件包开玩笑. `my-package`位于莫诺佩罗. 

```javascript
{
    "name": "Jest my-package",
    "type": "node",
    "request": "launch",
    "address": "localhost",
    "protocol": "inspector",
    "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/lerna",
    "runtimeArgs": [
        "exec",
        "--scope",
        "my-package",
        "--",
        "node"
    ],
    "args": [
        "${workspaceRoot}/node_modules/jest/bin/jest.js",
        "--runInBand",
        "--no-cache",
        "packages/my-package"
    ]
}
```

-   [`--runInBand`](https://facebook.github.io/jest/docs/en/cli.html#runinband)避免跨多个进程并行化测试
-   [`--no-cache`](https://facebook.github.io/jest/docs/en/cli.html#cache)有助于避免缓存问题

使用VisualStudio代码V1.19.3和JEST V22.1.4进行测试. 
