### nodejs中module.exports和exports的区别

先说一下它们之间的区别：

- exports只能通过语法来向外暴露内部变量：如exports.xxx = xxx;

- module.exports既可以通过语法，也可以直接赋值一个对象：

  1. module.exports.xxx = xxx;

  2. module.exports = {

     ​	xxx:xxx

     }

module.exports 其实和exports是同一个东西

~~~javascript
console.log(module.exports === exports) //true
~~~

node中规定：

- module.exports初始值为一个空对象{}
- exports是指向module.exports的引用;
- require返回的是module.exports而不是exports



所以使用exports不能直接为其赋值一个对象，因为重新为其赋一个对象，相当于改变了原来的引用，使得exports不再指向module.exports