# 古くなった JavaScript の書き方、これからの JavaScript の書き方まとめ

## 変数・リテラル

### 宣言
昔は var を使っていた。
```javascript
// OLD STYLE
var name = "yamamoto";
```
今後使うべきなのは const 。上書きがどうしても必要なところだけ let にする。
```javascript
// BETTER WAY
var name = "yamamoto";
```
