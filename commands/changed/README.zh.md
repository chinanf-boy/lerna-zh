
# `@lerna/changed`

> 列出自上一次标签发布以来更改的本地包

## 用法

产量`lerna changed`是下一个主题的包列表`lerna version`或`lerna publish`执行. 

```sh
$ lerna changed
package-1
package-2
```

**注: ** `lerna.json`配置为`lerna publish` *和* `lerna version`也影响`lerna changed`,例如`command.publish.ignoreChanges`.

## 选项

`lerna changed`支持所有支持的标志[`lerna ls`](https://github.com/lerna/lerna/tree/master/commands/list#options): 

-   [`--json`](https://github.com/lerna/lerna/tree/master/commands/list#--json)
-   [`-a`,`--all`](https://github.com/lerna/lerna/tree/master/commands/list#--all)
-   [`-l`,`--long`](https://github.com/lerna/lerna/tree/master/commands/list#--long)
-   [`-p`,`--parseable`](https://github.com/lerna/lerna/tree/master/commands/list#--parseable)

不像`lerna ls`然而,`lerna changed` **不**支持[过滤器选项](https://www.npmjs.com/package/@lerna/filter-options),因为过滤不支持`lerna version`或`lerna publish`.

`lerna changed`还支持所有支持的标志[`lerna version`](https://github.com/lerna/lerna/tree/master/commands/version#options),但唯一相关的是[`--ignore-changes`](https://github.com/lerna/lerna/tree/master/commands/version#--ignore-changes).
