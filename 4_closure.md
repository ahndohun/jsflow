# 4. closure

```js
// 4-1-1
function setName(name) {
  return function() {
    return name;
  }
}
var sayMyName = setName('고무곰');
sayMyName();
```

```js
// 4-1-2
function setCounter() {
  var count = 0;
  return function() {
    return ++count;
  }
}
var count = setCounter();
count();
```

```js
// 4-1-3
for(var i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i);
  }, 200 * i);
}

// 수정본

for(var i = 0; i < 10; i++) {
  (function(i){
    setTimeout(function() {
      console.log(i);
    }, 200 * i);
  })(i);
}
```


```html
<style>
#a, #b {
  margin: 10px;
  height: 0;
  padding: 20px;
  overflow: hidden;
  background-color: #ccc;
}
#a.open, #b.show {
  height: auto;
}
</style>

<div id="a">
  <p>I'm #a</p>
</div>
<div id="b">
  <p>I'm #b</p>
</div>
```

```js
function makeToggleClass(cls) {
  return function(e) {
    var classList = e.currentTarget.className
      ? e.currentTarget.className.split(' ')
      : [];
    var targetIndex = classList.findIndex(function(v){
      return v === cls
    });
    if(targetIndex >= 0) {
      classList.splice(targetIndex, 1);
    } else {
      classList.push(cls);
    }
    e.currentTarget.className = classList.join(' ');
  }
}

var openToggle = makeToggleClass('open');
var showToggle = makeToggleClass('show');

document.getElementById('a').addEventListener('click', openToggle);
document.getElementById('b').addEventListener('click', showToggle);
```

http://output.jsbin.com/codikekigo/
