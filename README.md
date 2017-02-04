## javascript面试题总结

<!-- MarkdownTOC -->

- [this](#问题1：this)   
- [call和apply](#jump2)     
- [内置方法](#jump3)     
- [闭包](#jump4)     
- [声明提前](#jump5)     
- [递归](#jump6)     

<!-- /MarkdownTOC -->

### 问题1：this

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

### <span id='jump2'>问题2：call()和apply()</span>

修复前一个问题，让最后一个console.log() 打印输出bryant      
**答案：**   
  
```js   
console.log(test.call(obj.prop))
console.log(test.apply(obj.prop))
```
追加提问，call和apply的区别

### <span id='jump3'>问题3：创建“内置”方法</span>

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

### <span id='jump4'>问题4: 闭包</span>
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
### <span id='jump5'>问题5: 声明提前</span>

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

### <span id='jump6'>问题6：递归</span>

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







