# 古くなった JavaScript の書き方、これからの書き方

## 変数・リテラル

### 宣言

昔は var を使っていた。

```javascript
// OLD STYLE
var name = 'yamamoto'
```

今後使うべきなのは const 。上書きがどうしても必要なところだけ let にする。

```javascript
// BETTER WAY
const name = 'yamamoto'
```

ただし、const で宣言しても、配列への再代入は禁止できないことに注意する。

```javascript
// NO ERROR
const arr = []
arr.push(1)
arr.push(2)
console.log(arr) // [1, 2]
```

再代入可能な 変数を宣言する時は let を使用する。

```javascript
let name = 'yamamoto'
name = '再代入 OK'
console.log(name) // 再代入 OK
```

## 変数のスコープ

```javascript
// OLD STYLE
for (var i = 0; i < 10; i++) {
  console.log(i) // -> 0, 1, 2, 3...
}
console.log(i) // -> 10
```

```javascript
// BETTER WAY
for (let i = 0; i < 10; i++) {
  console.log(i) // -> 0, 1, 2, 3...
}
console.log(i) // -VM47:4 Uncaught ReferenceError: i is not defined
```

## 文字列の結合

```javascript
// OLD STYLE
console.log('[Debug]:' + variable)
```

文字列テンプレートリテラルを使う。

```javascript
// BETTER WAY
console.log(`[Debug:] ${variable}`)
```

## オブジェクトのコピー

```javascript
// OLD STYLE
var destObj = {}
for (var key in srcObj) {
  if (srcObj.hasOwnProperty(key)) {
    destObj[key] = srcObj[key]
  }
}
```

ES6(ES2015) では Object.assign() というクラスメソッドが使える。

```javascript
// BETTER WAY
const destObj = {}
Objct.assign(destObj, srcObj)
```

ECMAScript 2018 では、オブジェクトのスプレッド演算子が使える。

```javascript
const destObj = { ...srcObj }
```

## クラス宣言

ES6(ES2015) でクラス構文が導入される前は、関数と prototype 属性でクラスを表現していた。

```javascript
// OLD TYPE クラス
function Dog() {
  this.name = 'chiwawa'
  this.bark = function () {
    return 'Woof!'
  }
}

// OLD TYPE 継承
Dog.prototype = new Animal()

// OLD TYPE メソッド
Dog.prototype.sayMyName = function () {
  console.log(this.name)
}

var chiwawa = new Dog()
chiwawa.sayMyName() // chiwawa
```

ES6(ES2015) 以降のクラスを使うと以下の通り。

```javascript
class Dog extends Animal {
    constructor() {
        this.name = 'chiwawa'
    }

    sayMyName() {
        console.log(this.name)
    }
}
```

## 関数宣言
