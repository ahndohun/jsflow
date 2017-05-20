# 2. 함수

## 2-1. 함수 (function)

```js
// 2-1-1
function sum(a, b) {
  return a + b;
}
console.dir(sum);

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

## 2-2. 함수선언문과 함수표현식 (function delcaration vs. function expression)

```js
// 2-2-1
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

## 2-3. 호이스팅

```js
// 2-3-1
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
var i = 0;
var timer = setInterval(function() {
  i = i + 1;
  if(i > 10) {
    clearInterval(timer);
    return;
  }
  console.log(i);
}, 200);

// -> 변수 분리

var i = 0;
var callbackFunc = function() {
  i = i + 1;
  if(i > 10) {
    clearInterval(timer);
    return;
  }
  console.log(i);
}
var timer = setInterval(callbackFunc, 200);
```

```js
// 2-6-2
var a = 1;
var b = 2;
function sum() {
  return a + b;
}

setTimeout(sum, 300);
document.getElementById('a').addEventListener('click', sum);
$('#a').on('click', sum);
```

```js
// 2-6-3
var callbacks = {
  logEachValues: function(v, i) {
    console.log(i, v);
  },
  mapToBeSquare: function(v, i) {
    return v * (v + i);
  }
};

var arr = [1, 2, 3];
arr.forEach(callbacks.logEachValues);

var res = arr.map(callbacks.mapToBeSquare));
console.log(res);
```

```js
// 2-6-4
Array.prototype.forEach = function(callback, thisArg) {
  var self = thisArg || this;
  for(var i = 0; i < this.length; i++) {
    callback.call(self, this[i], i, this);
  }
}
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
