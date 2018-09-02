
# `@lerna/changed`

> 列出自上一次标签发布以来更改的本地包

## 用法

`lerna changed`的输出,是包列表,给予`lerna version`或`lerna publish`执行的下一子项. 

```sh
$ lerna changed
package-1
package-2
```

**注:** `lerna.json`的配置影响了`lerna publish` *和* `lerna version`,同时也影响`lerna changed`, 例如`command.publish.ignoreChanges`.

## 选项

`lerna changed`支持[`lerna ls`](https://github.com/lerna/lerna/tree/master/commands/list#options)所有支持的标志: 

-   [`--json`](https://github.com/lerna/lerna/tree/master/commands/list#--json)
-   [`-a`,`--all`](https://github.com/lerna/lerna/tree/master/commands/list#--all)
-   [`-l`,`--long`](https://github.com/lerna/lerna/tree/master/commands/list#--long)
-   [`-p`,`--parseable`](https://github.com/lerna/lerna/tree/master/commands/list#--parseable)

然而,`lerna changed`不像`lerna ls`, **不**支持[过滤器选项](https://www.npmjs.com/package/@lerna/filter-options),因为过滤页不支持`lerna version`或`lerna publish`.

`lerna changed`还支持[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#options)所有支持的参数选项,但唯一相关的是[`--ignore-changes`](https://github.com/lerna/lerna/tree/master/commands/version#--ignore-changes).
