---
title: ES6
tags: javaScript
description: 学习使用 ES6 新特性
top_img: /img/cover/wyy2.png
date:
cover:基础
---



# 基础

## let 变量

1. 不能重复声明

2. 块级作用域

   ```javascript
   if(v > 10){
       let book = 1;
       console.log(book);
   }
   ```

   

3. 不存在变量提升

   ```JavaScript
   console.log(book);
   var book;  // var 变量会被浏览器先全部收集, 可在定义语句之前使用
   ```

4. 不影响作用域链

   ```JavaScript
   {
       let school = 'jxust';
       function f (){
           console.log(school); // 当前块作用域无 school 变量, 前往上级寻找
       }
       f();
   }
   ```

## const 变量

1. 必须赋初值

2. 格式: `const SCHOOL = "jxust"`, 一般命名全大写

3. 无法修改, 修改会报错

4. 块级作用域

5. 对于数组和对象的元素修改, 不算对常量的修改, 不报错

   ```javascript
   const TEAM = ['Tom', 'Mark'];
   TEAM.push('wx'); // TEAM 存储的是数组的地址, 不改变地址不算修改 TEAM
   ```

## 对象解构

```javascript
const student = {
    name: 'wx',
    age = 21,
    behaviour: function(){
        console.log("eat");
    }
}

let {behaviour} = student;
behaviour(); // 省却了student.behaviour();中的对象名称
```

## 模板字符串

1. 字符串中可出现换行符
2. 支持拼接字符串

```javascript
// 使用反引号``, 其中可出现换行符
let str = `<ul>
			<li> 213 </li>
			<li> 213 </li>
			</ul>`;

// 字符串拼接
let str1 = "qwqcoder";
let str2 = `${str1}爱吃烤肉`;
```

## 对象创建

```javascript
let name = 'qwqcoder'
let eat = function(){
    console.log('eat meat');
}
const student = {
    name,  // 本来是name: name,
    eat,	// 本来是 eat: eat,
    study(){ //本来是 study: function(){...}
        console.log('consitently study');
    }
}
```

## 箭头函数

1. `this` 值是静态的, 始终指向函数声明时所在作用域的 `this`
2. 不能作为构造函数实例化对象
3. 不能使用 `arguments` 变量

```javascript
// 常规函数声明
let fn1 = function(){
    console.log('fn1, 一般函数声明');
}
// 箭头函数声明
let fn2 = (a, b) => {
    return a + b;
}
// 调用
let res = fn2(1, 13);
console.log(res);

// ----------------------------------------------------
// 箭头函数的 this 是静态的, 固定指向函数声明时所在作用域的 this
function getName(){
    console.log(this.name);
}
let getName2 = () => {
    console.log(this.name);
}
window.name = 'qwqcoder';
const school = {
    name: 'jxust'
}

// window 下直接调用
getName(); // 输出 qwqcoder
getName2(); // 输出 qwqcoder
// 改变调用者
getName.call(school); // 输出 jxust
getName.call(school); // 输出 qwqcoder


// ----------------------------------------------------
// 箭头函数, 不能作为构造函数实例化对象
let Person = (name, age) => {
    this.name = name;
    this.age = age;
}
let me = new Person('qwqcoder', 21); // 报大错😋

// ---------------------------------------------------
let fn3 = () => {
    console.log(arguments);
}
fn3(1, 2, 3); // 报大错.😋

// ----------------------------------------------------
// 简化声明语法
// 1. 只有一个形参
let pow = n => {  // 本来是 let add = (n) => {}
    return n * n;
}
// 2. 函数体就就一条语句, 此时可省略花括号, 若省略, 则return也必须省略
let add = n => n + n;
```

## rest 参数

```javascript
function data(...args){
    console.log(args);
}
data(1, 2, 3);

// rest 
function fn(a, b, ...args){
    console.log(a);
    console.log(b);
    console.log(args);
}
fn(1,2,3,4,5,6);

// ----------------------
// 应用1
function fn1(){
    console.log(arguments);
}
let a = ['qwqcoder', 'orzcoder', 'buskcoder']
fn1(...a); // 实参被 rest 变量从一个数组转化成多个变量

// 应用2
// 合并数组
const array1 = [1, 2];
const array2 = [3, 4];
const new_array = [...array1, ...array2];
const num_1234 = array1.concat(array2);
// 克隆数组
const colon_array = [...array1]; // 若存在引用类型, 仍是浅拷贝

```

## Symbol 变量

```javascript
// 创建 Symbol
let s = Symbol();
let s2 = Symbol("qwqcoder");

```





# 示例

