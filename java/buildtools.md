# 1 Maven
```text
jar包仓库管理
每个jar包在仓库里都有唯一的id。由三部分组成：group id、artifact id和version。

注意：
snapshot版本的jar包是不稳定版本。

常用命令：
clean
compile
test
package
install

mvn dependency:tree

常用插件：
maven是一套框架，所有具体任务都是插件完成的。除了核心的编译打包插件，还有非常多别的目的的插件。
```