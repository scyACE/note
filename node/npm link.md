## npm link

### 功能

- 在本地开发npm模块中，我们可以通过npm link命令，将npm模块链接到对应的模块中去，方便对模块进行调试和测试。

### 使用方法

#### 创建链接

首先我们有两个模块，一个是需要我们需要开发的npm模块develop-module，另一个则是要运行npm包的的项目

#### 官网解释

首先，包文件夹中的npm link会在全局文件夹{prefix}/lib/node_modules/<package>中创建一个符号链接，它链接到执行npm link命令的包。它还会将包中的任何bin链接到{prefix}/bin/{name}。注意，npm link使用了全局前缀。

#### 具体代码实现

~~~linux
cd develop-module
npm link
~~~



这时develop-module模块会被加载到本地node_modules/lib里

在要运行的npm的项目下

~~~
npm link develop-module
~~~



npm包就被加载到项目的node_modules下了，可以被当前项目引用，而且当npm模块进行修改时，当前项目也会获得最新的代码

卸载

~~~
npm unlink <package> //卸载指定npm包
~~~

