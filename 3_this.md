# 3. this

## 3-1. 전역컨텍스트에서의 this

```js
/* 브라우저 콘솔에서 */
console.log(this);

/* node.js에서 */
console.log(this.process.title);
```


## 3-2. 함수 내부에서의 this

```js
function a() {
  console.log(this);
}
a();
```


## 3-3. 메소드 호출시의 this

```js
var a = {
  b: function() {
    console.log(this);
  }
}
a.b();
```

```js
var a = {
  b: {
    c: function() {
      console.log(this);
    }
  }
}
a.b.c();
```


## 3-4. 명시적인 this 바인딩

```js
function a(x, y, z) {
  console.log(this, x, y, z);
}
var b = {
  c: 'eee'
}

a.call(b, 1, 2, 3);

a.apply(b, [1, 2, 3]);

var c = a.bind(b);
c(1, 2, 3);

var d = a.bind(b, 1, 2);
d(3);
```


## 3-5. 콜백함수에서의 this

```js
var dom;
document.body.innerHTML += '<div id="a">클릭하세요</div>';
dom = document.getElementById('a');
dom.addEventListener('click', function() {
  console.dir(this);
});
```

```js
var callback = function() {
  console.dir(this);
};
var obj = {
  a: 1,
  b: function(cb) {
    cb();
  }
};
obj.b(callback);
```

```js
var callback = function() {
  console.dir(this);
};
var obj = {
  a: 1,
  b: function(cb) {
    cb && cb.call(this);
  }
};
obj.b(callback);
```

```js
setTimeout(callback, 100);

setTimeout(callback.bind(obj), 100);
```

```js
var dom;
var callback = function() {
  console.log(this);
};
var obj = { a: 1 };

document.body.innerHTML += '<div id="a">클릭하세요</div>';
dom = document.getElementById('a');

dom.addEventListener('click', callback.bind(obj));
```


## 3-6. 생성자함수에서의 this

```js
function Person(n, a) {
  this.name = n;
  this.age = a;
}
var gomugom = new Person('고무곰', 30);
console.log(gomugom);
```


## 3-7. 내부함수에서의 this의 문제점과 우회법

```js
var a = 10;
var obj = {
  a: 20,
  b: function() {
    console.log(this.a);

    function c() {
      console.log(this.a);
    }
    c();
  }
}
obj.b();

// 우회법

var a = 10;
var obj = {
  a: 20,
  b: function() {
    var self = this;
    console.log(this.a);

    function c() {
      console.log(self.a);
    }
    c();
  }
}
obj.b();
```


## 3-8. 정리

- 전역컨텍스트에서 this
- 함수 내부에서의 this
- 메소드 호출시의 this
- 명시적인 this 바인딩 방법 세가지
- 콜백함수에서의 this
- 생성자함수에서의 this
- 지역함수에서의 this 우회법
