<!-- MarkdownTOC -->

- [javascript面试题总结](#javascript面试题总结)      
  - [this](#this)       
  - [call和apply](#call和apply)  
  - [创建内置方法](#创建内置方法) 
  - [闭包](#闭包)
  - [声明提前](#声明提前) 
  - [递归](#递归)
  - [bind函数实现](#bind函数实现) 
  - [once函数](#once函数) 
  - [数组乱序](#数组乱序) 

<!-- /MarkdownTOC -->

## javascript面试题总结

### this

```js
var fullname = 'paul';

var obj = {

  fullname: 'kobe',

  prop: {

    fullname: 'bryant',

    getFullname: function() {

      return this.fullname;

    }
  }
};

console.log(obj.prop.getFullname());

var test = obj.prop.getFullname;

console.log(test());
```
**答案：** 代码输出：bryant 和 paul

### call和apply

修复前一个问题，让最后一个console.log() 打印输出bryant      
**答案：**   
  
```js   
console.log(test.call(obj.prop))
console.log(test.apply(obj.prop))
```
追加提问，call和apply的区别

### 创建内置方法

给String对象定义一个repeatify方法。该方法接收一个整数参数，作为字符串重复的次数，最后返回重复指定次数的字符串。例如：

```js
var str = 'hello';
console.log(str.repeatify(3));
```

输出应该是: hellohellohello

**答案:**

```js
String.prototype.repeatify = String.prototype.repeatify || function (times) {
  var str = '';

  for (var i = 0; i < times; i++) {
    str += this;
  }

  return str;
};
```

### 闭包
```js
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
    console.log(0);
  }, 1000);
  console.log(5);
}
```
**答案：**       
5    
5    
5       
3    
0    
3    
0    
3     
0

追加提问：如果想输出：    
5     
5     
5      
0     
0      
1     
0      
2      
0     
**答案1：** 严格模式下，var改成let     
**答案2：** 闭包    

```js
for(var i = 0; i < 3; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
      console.log(0);
    }, 1000);
  })(i)

  console.log(5)
}
```
### 声明提前

```js
function test() {
  console.log(a);
  console.log(foo());

  var a = 1;
  function foo() {
    return 2;
  }
}

test();
```
**答案：** undefined 2
变量和函数的声明都被提前至函数体的顶部，而同时变量并没有被赋值。因此，当打印变量a时，它虽存在于函数体（因为a已经被声明），但仍然是undefined。换句话说，上面的代码等同于下面的代码：

```js
function test() {
  var a;
  function foo() {
    return 2;
  }
  
  console.log(a);
  console.log(foo());
  
  a = 1;
}

test();
```

### 递归

```js
function test(n) {
  if (n == 0) {
    return 0;
  }
  if (n == 1) {
    return 1;
  }
  return test(n-1) + test(n-2);
}
console.log(test(5));
```
改成非递归的形式：

```js
function test2(n) {
  var arr = [0, 1];

  for (var i = 2; i <= n; i++) {
    arr.push(arr[i-1] + arr[i-2]);
  }
  return arr[n];
}
console.log(test2(5))


function test3(n) {
  var obj = {};
  obj[0] = 0;
  obj[1] = 1;

  for(var i = 2; i <=n; i++) {
    obj[i] = obj[i-1] + obj[i-2];
  }
  return obj[n];
}
console.log(test3(5))
```

### bind函数实现

```js
Function.prototype.bind = function(context) {
  let self = this;
  return function() {
    return self.apply(context, arguments);
  }
}

let obj = {
  num: 1,
  getNum: function() {
    return this.num;
  }
};

let objj = {
  num: 2
};

console.log(obj.getNum.bind(objj)());  // 2
```

### once函数

*重点在于执行完一次后将function置为null*

```js
function once(fn, context) {
  let result;
  return function() {
    if (fn) {
      result = fn.apply(context || this, arguments);
      fn = null;
    }
    return result;
  }
}

var execOnce = once(function() {
  console.log('exec success');
});

execOnce();  // "exec success"
execOnce();  // nothing
```

### 数组乱序

**又称洗牌**

```js
function shuffle(a) {
  var b = [];
  while(a.length) {
    var index = Math.floor(Math.random() * a.length);
    b.push(a[index]);
    a.splice(index, 1);
  }
  return b;
}

var a = [1,2,3,4,5,6];
console.log(shuffle(a));  // [6,2,1,5,3,4]
```








