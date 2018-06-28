##### 1. 什么是 Thunk 函数
Thunk 函数来源于编译器的求值策略 ———  传值调用和传名调用.
```
var a = 1;

function f(x){
  return x * 2;     
}

f(a + 6)
```
上面的函数结果该如何计算？如果是先计算 :<br>
<code>a + 6 => f(6) => 6 * 2</code>  是传值调用<br>
<code>a + 6 => f(a+6) => (a + 6) * 2 </code> 是传名调用; 两种调用方法各有优缺点.<br>

要想弄清楚什么是 Thunk 函数，我们得从传名调用说起：<br>
编译器传名调用的实现，主要是先将参数放到一个临时函数中，再将临时参数以参数的形式传入目标函数中，而这个临时函数就是 Thunk 函数. 所谓 Thunk 函数就是对传名调用的某种实现.
```
function f(m){
  return m * 2;     
}

f(a + 5);

// 等同于

var thunk = function () {
  return a + 5;
};

function f(thunk){
  return thunk() * 2;
}
```

##### 2. Thunk 函数在 JS 中应用
JS实现的是传值调用，Thunk 在 JS 中被用来处理多参数函数，替代的多参数函数中的一个个参数，把目标参数替换成单参数函数，并且只接受回调函数作为参数。下面是 Thunkify 模块的源代码( Thunkify 配合 co 使用，将function(param,callback), 风格的函数，转化为 thunky 风格的函数)

```
function thunkify(fn){
  return function(){
    var args = new Array(arguments.length);
    var ctx = this;

    for(var i = 0; i < args.length; ++i) {
      args[i] = arguments[i];
    }

    return function(done){
      var called;

      args.push(function(){
        if (called) return;
        called = true;
        done.apply(null, arguments);
      });

      try {
        fn.apply(ctx, args);
      } catch (err) {
        done(err);
      }
    }
  }
};  
```
```
function f(a, b, callback){
  var sum = a + b;
  callback(sum);
  callback(sum);
}

var ft = thunkify(f);
ft(1, 2)(console.log); //3
```

