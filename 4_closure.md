# 4. closure

## 4-1. closure?
```js
// 4-1-1
function a() {
  var x = 1;
  function b() {
    console.log(x);
  }
  b();
}
a();
console.log(x);

// 변경
function a() {
  var x = 1;
  return function b() {
    console.log(x);
  }
}
var c = a();
c();

// 다시 변경
function a() {
  var _x = 1;
  return {
    get x() { return _x; },
    set x(v) { _x = v; }
  }
}
var c = a();
console.log(c.x);
c.x = 10;
console.log(c.x);
```

```js
// 4-1-2
function setName(name) {
  return function() {
    return name;
  }
}
var sayMyName = setName('고무곰');
sayMyName();
```

```js
// 4-1-3
function setCounter() {
  var count = 0;
  return function() {
    return ++count;
  }
}
var count = setCounter();
count();
```

## 4-2. closure로 private member 만들기

```js
// 4-2-1
var car = {
  fuel: 10, // 연료 (l)
  power: 2, // 연비 (km / l)
  total: 0,
  run: function(km) {
    var wasteFuel = km / this.power;
    if(this.fuel < wasteFuel) {
      console.log('이동 불가');
      return;
    }
    this.fuel -= wasteFuel;
    this.total += km;
  }
};

// 변경후
var createCar = function(f, p) {
  var fuel = f;
  var power = p;
  var total = 0;
  return {
    run: function(km) {
      var wasteFuel = km / power;
      if(fuel < wasteFuel) {
        console.log('이동 불가');
        return;
      }
      fuel -= wasteFuel;
      total += km;
    }
  }
};
var car = createCar(10, 2);
```

```js
-->
// 4-2-2
const Queue = (() => {
  const ARRAY = Symbol('ARRAY');
  return class {
    constructor(...v) {
      this[ARRAY] = [...v];
    }
    push(...v) {
      return this[ARRAY].push(...v);
    }
    pop() {
      return this[ARRAY].shift();
    }
    get length() {
      return this[ARRAY].length;
    }
  }
})();

const arr = new Queue();
arr.push(1, 2, 3);
console.log(arr.pop());
```
