# 5. prototype

## 5-1. prototype과 constructor, `__proto__`

```js
// 5-1-1.
function Person(n, a) {
  this.name = n;
  this.age = a;
}
var gomu = new Person('고무곰', 30);

var gomuClone = new gomu.constructor('고무곰_클론', 25);

var gomuProto = Object.getPrototypeOf(gomu);
var gomuClone2 = new gomuProto.constructor('고무곰_클론2', 20);

var gomuClone3 = new Person.prototype.constructor('고무곰_클론3', 15);

var gomuClone4 = new gomu.__proto__.constructor('고무곰_클론4', 10);
```

```js
Object.getPrototypeOf([instance])
[CONSTRUCTOR].prototype
[instance].__proto__
[instance]
```

```js
[CONSTRUCTOR]
[CONSTRUCTOR].prototype.constructor
(Object.getPrototypeOf([instance])).constructor
[instance].__proto__.constructor
[instance].constructor
```

### 5-2. 메소드 상속 및 동작 원리

```js
// 5-2-1.
function Person(n, a) {
  this.name = n;
  this.age = a;
}

var gomu = new Person('고무곰', 30);
var iu = new Person('아이유', 25);

gomu.setOlder = function() {
  this.age += 1;
}
gomu.getAge = function() {
  return this.age;
}
iu.setOlder = function() {
  this.age += 1;
}
iu.getAge = function() {
  return this.age;
}

console.dir(gomu);
console.dir(iu);
gomu.setOlder();
gomu.getAge();
iu.setOlder();
iu.getAge();
```

```js
// 5-2-2.
function Person(n, a) {
  this.name = n;
  this.age = a;
}
Person.prototype.setOlder = function() {
  this.age += 1;
}
Person.prototype.getAge = function() {
  return this.age;
}

var gomu = new Person('고무곰', 30);
var iu = new Person('아이유', 25);

console.dir(gomu);
console.dir(iu);

gomu.setOlder();
gomu.getAge();

iu.setOlder();
iu.getAge();
```

```js
gomu.setOlder();
gomu.getAge();
gomu.__proto__.setOlder();
gomu.__proto__.getAge();

Person.prototype.age = 100;

gomu.setOlder();
gomu.getAge();
gomu.__proto__.setOlder();
gomu.__proto__.getAge();
```


## 5-3. prototype chaining

```js

// 5-3-0
var arr = [1, 2, 3];
console.log(arr.toString());

arr.toString = function() {
  return this.join(',');
}
Array.prototype.toString = function() {
  return '[' + this.join(', ') + ']';
}

console.log(arr.toString());
console.log(arr.__proto__.toString.call(arr));
console.log(arr.__proto__.__proto__.toString.call(arr));

```


```js
// 5-3-1
var obj = {
  a: 1,
  b: {
    c: 'c'
  }
};
console.log(obj.toString());
```

```js
// 5-3-2
var obj = {
  a: 1,
  b: {
    c: 'c'
  },
  toString: function() {
    var res = [];
  	for(var key in this) {
  		res.push(key + ': ' + this[key].toString());
      }
  	return '{' + res.join(', ') + '}';
  }
};
console.log(obj.toString());

// 수정본

Object.prototype.toString = function() {
	var res = [];
	for(var key in this) {
		res.push(key + ': ' + this[key].toString());
    }
	return '{' + res.join(', ') + '}';
}
var obj = {
  a: 1,
  b: {
    c: 'c'
  }
};
console.log(obj.toString());
```

```js
// 5-3-3
var obj = {
  a: 1,
  b: {
    c: 'c'
  },
  d: [5, 6, 7],
  e: function(){}
};
Object.prototype.toString = function() {
	var res = [];
	for(var key in this) {
		res.push(key + ': ' + this[key].toString());
    }
	return '{' + res.join(', ') + '}';
}
Array.prototype.toString = function() {
  return '[' + this.join(', ') + ']';
}
console.log(obj.toString());
```
