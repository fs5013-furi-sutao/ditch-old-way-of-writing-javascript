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
console.log(arr) // -> [1, 2]
```

再代入可能な 変数を宣言する時は let を使用する。

```javascript
let name = 'yamamoto'
name = '再代入 OK'
console.log(name) // -> 再代入 OK
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
console.log(i) // Uncaught ReferenceError: i is not defined
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

アロー関数がなかったと時の書き方。

```javascript
// OLD STYLE
function name(arg) {
  // body
}
```

アロー関数を使うとこう。

```javascript
// BETTER WAY
const name = (arg) => {
  // body
}
```

## 即時関数

即時関数という書き方があったが、今ではあえて利用する場面は稀となっている。

```javascript
// OLD STYLE
var lib = function () {
  var libBody = {}

  var localVariable

  libBody.method = function () {
    console.log(localVariable)
  }
  return libBody
}
```

## 非同期処理

Promise が登場する前は、いわゆるコールバック地獄と呼ばれる状況が、非同期処理で起きていた。

```javascript
// OLD STYLE
const fs = require('fs')

fs.readFile('data1.txt', function (data1) {
  fs.readFile('data2.txt', function (data2) {
    fs.readFile('data3.txt', function (data3) {
      fs.readFile('data4.txt', function (data4) {
        fs.readFile('data5.txt', function (data5) {
          console.log(data1 + data2 + data3 + data4 + data5)
        })
      })
    })
  })
})
```

非同期処理の待ちに Promise を使うようになるとこうなる。

```javascript
// BETTER WAY
const getData = (url) => {
  fetch(url)
    .then((res) => {
      return res.jsonz()
    })
    .then((json) => {
      console.log(json.data)
    })
}
```

今時は async/await キーワードをアロー関数の前に付与する。

async が付くと、その関数が Promise オブジェクトを返すようになる。

```javascript
// BETTER WAY
const fetchData = async (url) => {
  const res = await fetch(url)
  const json = await res.json()
  console.log(json.data)
}
```

上記のように、多くの例では返り値を変数に入れるが、以下の sleep のように必ずしも代入が必要とは限らない。
```javascript
const sleep = time => {
    return new Promise(resolve => {
        setTimeout(resolve, time)
    })
}

await sleep(100)
```

## apply()
昔は、関数に引数セットを配列で渡したいときは apply() メソッドを使っていた。
```javascript
// OLD STYLE
function f(a, b, c) {
    console.log(a, b, c);
}

f.apply(null, [1, 2, 3]);  // 1 2 3
```

今は配列展開するスプレッド演算子を使えば同じことができる。
```javascript
// BETTER WAY
const f = (a, b, c) => {
    console.log(a, b,  c)
}

f(...[1, 2, 3])  // 1, 2, 3
```
関数の引数を可変長配列にしたい時は以下のように書ける。
```javascript
const f = (a, b, ...c) => {
    console.log(a, b, c)
}

f(1, 2, 3, 4, 5, 6)  // 1 2 [ 3, 4, 5, 6 ]
```

## デフォルト引数
JavaScript は、宣言された引数を付けずに呼び出すこともでき、その場合には undefined が設定される。
```javascript
// OLD STYLE
function f(a, b, c) {
    if (c === undefined) {
        c = 'default value'
    }
}
```

現在は関数宣言のところにデフォルト引数を書くことができる。
```javascript
// CURRENT STYLE
const f = (name='chiwawa', gender='female') => {
    console.log(name, gender)
}

f()  // chiwawa female
```
配列やオブジェクトは、分割代入する機能が増えたので、それと組み合わせると、オブジェクトで柔軟にパラメータを受け取りデフォルト値も同時に設定することもできる。
```javascript
const f = ({name='chiwawa', gender='female'}={}) => {
    console.log(name, gender)
}

f({ id: '11', gender: 'male'})  // chiwawa male
```
次のように分割代入することもできる。
```javascript
const data = {
    age: 14,
    gender: 'female', 
};

const {name='John Doe', age='unknown', gender='male'} = data;
console.log(`私の名前は ${name} です. ${age} 歳です. ${gender} です`);
// 私の名前は John Doe です. 14 歳です. female です
```

## 分割代入
分割代入を使えば、中間の変数を省略して、欲しいものだけを取得することができる。
```javascript
// OLD STYLE
var path = require('path')
var readFileSync = require('fs').readFileSync
var writeFileSync = require('fs').writeFileSync
```
```javascript
// BETTER WAY
const { join } = require('path')
import { readFilesync, writeFileSync } from 'fs'
```
