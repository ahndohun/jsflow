# 2. 함수

## 2-1. 함수 (function)

```js
// 2-1-1
function sum(a, b) {
  return a + b;
}

sum(1, 2);

var res = sum(2, 3);

console.log(res);
```

```js
// 2-1-2
function sum(a, b) {
  console.log(sum.caller);
  console.log(arguments);
  return a + b;
}
function callFunc() {
  sum(1, 2);
}
sum(10, 20, 30, 40);
callFunc();
```

## 2-2. 호이스팅

```js
// 2-2-1
console.log(a());
console.log(b());
console.log(c());

function a() {
  return 'a';
}
var b = function bb() {
  return 'bb';
}
var c = function() {
  return 'c';
}

// 재구성 결과

function a() {
  return 'a';
}
var b;
var c;

console.log(a());
console.log(b());
console.log(c());

b = function bb() {
  return 'bb';
}
c = function() {
  return 'c';
}
```


## 2-3-1. 함수선언문과 함수표현식 (function delcaration vs. function expression)

```js
// 2-3-1
function a() {
  return 'a';
}
var b = function bb() {
  return 'bb';
}
var c = function() {
  return 'c';
}
```

```js
// 2-3-2
function sum(a, b) {
  return a + ' + ' + b + ' = ' + (a + b);
}
sum(1, 2);

/* ... 중략 ... */

function sum(a, b) {
  return a + b;
}
sum(3, 4);


// 함수표현식의 경우

var sum = function(a, b) {
  return a + ' + ' + b + ' = ' + (a + b);
}
sum(1, 2);

/* ... 중략 ... */

var sum = function(a, b) {
  return a + b;
}
sum(3, 4);
```


## 2-4. 함수 스코프, 실행컨텍스트

```js
// 2-4-1
var a = 1;
function outer() {
  console.log(a);

  function inner() {
    console.log(a);
    var a = 3;
  }

  inner();

  console.log(a);
}
outer();
console.log(a);
```

## 2-5. 메소드 (method)

```js
// 2-5-1
var obj = {
  a: 1,
  b: function bb() {
    console.log(this);
  },
  c: function() {
    console.log(this.a);
  }
};

obj.b();
obj.c();

console.dir(obj.b);
console.dir(obj.c);
```

## 2-6. 콜백함수 (callback)

```js
// 2-6-1
setInterval(function () {
  console.log('1초마다 실행될 겁니다.');
}, 1000);
```

```js
var cb = function() {
  console.log('1초마다 실행될 겁니다.');
};
setInterval(cb, 1000);
```

```js
// 2-6-2
var arr = [1, 2, 3, 4, 5];
var entries = [];
arr.forEach(function(v, i) {
  entries.push([i, v, this[i]]);
}, [10, 20, 30, 40, 50]);
console.log(entries);

[ [0, 1, 10], [1, 2, 20], [2, 3, 30], [3, 4, 40], [4, 5, 50] ]
```

```js
// 2-6-3
Array.prototype.forEach = function(callback, thisArg) {
  var self = thisArg || this;
  for(var i = 0; i < this.length; i++) {
    callback.call(self, this[i], i, this);
  }
}
```

```js
var arr = [1, 2, 3, 4, 5];
arr.forEach(function(index, value) {
  console.log(index, value);
});
```


```js
// 2-6-4
document.body.innerHTML = '<div id="a">abc</div>';
function cbFunc(x) {
  console.log(this, x);
}
document.getElementById('a')
  .addEventListener('click', cbFunc);
$('#a').on('click', cbFunc);

<div id="a"></div>
```

```js
// 2-6-5
var arr = [1, 2, 3, 4, 5];
var obj = {
  vals: [1, 2, 3],
  logValues: function(v, i) {
    if(this.vals) {
      console.log(this.vals, v, i);
    } else {
      console.log(this, v, i);
    }
  }
};

obj.logValues(1, 2);
arr.forEach(obj.logValues);
```



## 2-7. 생성자함수 (constructor)

```js
// 2-7-1
function Person(name, age) {
  this.name = name;
  this.age = age;
}
var gomugom = new Person('고무', 20);

console.dir(gomugom);
console.dir(Person);
```

```js
// 2-7-2
function Person(name, age) {
  this.name = name;
  this.age = age;
  return name + ', ' + age;
}
var gomu1 = new Person('고무', 10);
var gomu2 = Person('곰', 20);

console.dir(gomu1);
console.dir(gomu2);
```
