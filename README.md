# 古くなった JavaScript の書き方、これからの書き方

## 変数・リテラル

### 宣言
昔は var を使っていた。
```javascript
// OLD STYLE
var name = "yamamoto"
```
今後使うべきなのは const 。上書きがどうしても必要なところだけ let にする。
```javascript
// BETTER WAY
const name = "yamamoto";
```
ただし、const で宣言しても、配列への再代入は禁止できないことに注意する。
```javascript
// NO ERROR
const arr = [];
arr.push(1);
arr.push(2);
console.log(arr);  // [1, 2]
```
再代入可能な 変数を宣言する時は let を使用する。
```javascript
let name = "yamamoto";
name = "再代入 OK";
console.log(name);  // 再代入 OK
```

## 変数のスコープ
```javascript
// OLD STYLE
for (var i = 0; i < 10; i++) {
    console.log(i);  // -> 0, 1, 2, 3...
}
console.log(i);  // -> 10
```

```javascript
// BETTER WAY
for (let i = 0; i < 10; i++) {
    console.log(i);  // -> 0, 1, 2, 3...
}
console.log(i);  // -> 10
```

## 文字列 → 数値の変換
