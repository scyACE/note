### Symbol

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JS语言的第七种数据类型，是一种类似于字符串的数据类型

- 特点

  - Symbol的值是唯一的，用来解决命名冲突问题
  - Symbol的值不能和其他数据一起进行运算
  - Symbol定义的对象属性不能使用for...in循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名

  ~~~javascript
  //创建Symbol
  let s = Symbol();
  
  console.log(s,typeof s);//Symbol() "symbol"
  
  let s2 = Symbol('孙悟空');
  let s3 = Symbol('孙悟空');
  
  console.log(s2 === s3);// false
  
  //通过Symbol.for创建
  let s4 = Symbol.for('孙悟空');
  let s5 = Symbol.for('孙悟空');
  
  console.log(s4,typeof s4);//Symbol(孙悟空) "symbol"
  console.log(s4 === s5);// true
  ~~~

