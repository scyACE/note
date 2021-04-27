### js模块化

#### js模块化历史

- ##### 最早时候，引用js文件 

~~~html
<script src=''></script>
<script src=''></script>
<script src=''></script>
~~~

~~~javascript
function fun1(){

}
function fun2(){

}
~~~



需要把每一个js放在顺序加载，如果js相互之间有依赖关系，则被依赖的文件要先加载，但是每个js包中对外的信息的暴露的，随时都可以修改，而且会污染全局变量

- ##### 通过一个对象引入

~~~javascript
let obj = {
  msg: "msg",
  say:function() {
    console.log(this.msg);
  },
};
~~~

在js中调用

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./module.js"></script>
</head>
<body>
    <script>
      obj.say();// msg
      obj.msg = 'change';
      obj.say();// change
    </script>
</body>
</html>
~~~

可以看到，虽然是通过一个对象引入的，但是还是可以直接修改到方法内部的属性

- ##### IIFE模式：匿名函数自调用（闭包）

~~~javascript
(function(window){
    let msg = 123;
    function log(){
        console.log(msg);
    }
    window.log = log;
})(window);
~~~

在js中调用

~~~
<script>
     log();//123
</script>
~~~

**node基于 Commonjs规范**

##### Commonjs：

1. 说明：

   - 每一个文件都可以当作一个模块
   - 在服务端：模块的加载时运行时同步加载的
   - 在浏览器端：模块需要提前编译打包处理

2. 基本语法

   1.暴露模块：

   - module.exports = xxx;
   - exports.xxx = xxx;

3. 引入模块
   
   - require(xxx)

##### AMD：

1. 说明：

   - 专门用于浏览器端，模块的加载是异步的

2. 基本语法

   1. 定义暴露模块：

   - 定义没有依赖的模块

     ~~~javascript
     define(function(){
       return 模块;
     })
     ~~~

   - 定义有依赖的模块

     ~~~javascript
     define(["module1", "module1"], function (m1, m2) {
         return 模块;
     });
     ~~~

3. 实现：

   - RequireJS  [官网](https://requirejs.org/docs/start.html)

4. 使用：

   目录结构

   ~~~
   ├── index.html
   ├── modules
   │   ├── module1.js
   		└── module2.js
   └── scripts
       ├── main.js
       └── require.js
   ~~~

   ~~~javascript
   //module2.js 定义没有依赖的模块
   define(function () {
     let showMessage = function () {
       return "module2.message";
     };
     return {
       showMessage,
     };
   });
   ~~~

   ~~~JavaScript
   //module1.js 定义有依赖的模块
   define(['module2'],function (module2) {//也可以直接使用路径  是以主文件为base的路径，不是当前文件 ['../modules/module2']
     let showMessage = function () {
       console.log("module1.message," + module2.showMessage());
     };
     return {
       showMessage
     };
   });
   ~~~

   ~~~javascript
   //main.js 引入以来的主文件
   (function(){
       require.config({
           paths:{
               module1:'../modules/module1',
               module2:'../modules/module2'
           }
       })
       
       require(['module1'],function(module1){
           module1.showMessage();//module1.message,module2.message
       })
   }())
   ~~~

   ~~~html
   //index.html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
       <!-- src为引入的require.js文件 data-main 是主入口 不能添加后缀-->
       <script data-main="./scripts/main" src="./scripts/require.js"></script> 
   </head>
   <body>
   </body>
   </html>
   ~~~

   

##### CMD(了解)

#####  <font color='red'>ES6:</font>

1. 说明：
   - 依赖模块需要编译打包处理
2. 语法：
   - 导出模块：export
   - 引入模块：import

~~~
//报错
export 1;

//报错
var m = 1;
export m;
~~~

