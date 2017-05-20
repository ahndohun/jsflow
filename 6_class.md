# 6. prototype chaining을 활용한 Class 구현

## 6-1. static 메소드 및 static 프로퍼티

> presentation

```js
function Person(name, age) {
  this._name = name;
  this._age = age;
}
Person.getInformations = function(instance) {
  return {
    name: instance._name,
    age: instance._age
  };
}
Person.prototype.getName = function() {
  return this._name;
}
Person.prototype.getAge = function() {
  return this._age;
}

var gomu = new Person('고무', 30);

console.log(gomu.getName());
console.log(gomu.getAge());

console.log(gomu.getInformations(gomu));

console.log(Person.getInformations(gomu));
```


## 6-2. Class 상속구조

```js
// 6-2-1
function Person(name, age) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
}
Person.prototype.getName = function() {
  return this._name;
}
Person.prototype.getAge = function() {
  return this._age;
}

function Employee(name, age, position) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
  this._pos = position || '직책모름';
}
Employee.prototype.getName = function() {
  return this._name;
}
Employee.prototype.getAge = function() {
  return this._age;
}
Employee.prototype.getPosition = function() {
  return this._pos;
}
```

```js
// 6-2-2
function Person(name, age) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
}
Person.prototype.getName = function() {
  return this._name;
}
Person.prototype.getAge = function() {
  return this._age;
}

function Employee(name, age, position) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
  this._pos = position || '직책모름';
}

Employee.prototype = new Person();
Employee.prototype.constructor = Employee;
Employee.prototype.getPosition = function() {
  return this._pos;
}

var gomu = new Employee('고무', 30, 'CEO');
console.dir(gomu);

console.log(
  gomu.getName(),
  gomu.getAge(),
  gomu.getPosition()
);
console.log(
  gomu.__proto__.getName(),
  gomu.__proto__.getAge(),
  gomu.__proto__.getPosition()
);
```

```js
// 6-2-3
function Person(name, age) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
}
Person.prototype.getName = function() {
  return this._name;
}
Person.prototype.getAge = function() {
  return this._age;
}

function Employee(name, age, position) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
  this._pos = position || '직책모름';
}

function Bridge() {}
Bridge.prototype = Person.prototype;
Employee.prototype = new Bridge();
Employee.prototype.constructor = Employee;

Employee.prototype.getPosition = function() {
  return this._pos;
}

var gomu = new Employee('고무', 30, 'CEO');
console.log(gomu.getName(), gomu.getAge(), gomu.getPosition());

console.log(gomu.__proto__.getName(), gomu.__proto__.getAge(), gomu.__proto__.getPosition());
```

```js
// 6-2-4
var extendClass = (function() {
  function Bridge(){}
  return function(Parent, Child) {
    Bridge.prototype = Parent.prototype;
    Child.prototype = new Bridge();
    Child.prototype.constructor = Child;
    Child.superClass = Parent.prototype;
  }
})();

function Person(name, age) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
}
Person.prototype.getName = function() {
  return this._name;
}
Person.prototype.getAge = function() {
  return this._age;
}
function Employee(name, age, position) {
  this._name = name || '이름없음';
  this._age = age || '나이모름';
  this._pos = position || '직책모름';
}

extendClass(Person, Employee);
Employee.prototype.getPosition = function() {
  return this._pos;
}
var gomu = new Employee('고무', 30, 'CEO');
console.dir(gomu);
```
