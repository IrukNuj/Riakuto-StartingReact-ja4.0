# [りあクト！ TypeScriptで始めるつらくないReact開発 第4版【① 言語・環境編】](https://booth.pm/ja/items/2368045)

読みます。

## 適宜メモ

```javascript
React の中級者以上の方にも楽しみながら読んでいただける内容になっています。React
を始めとしたフロントエンド技術のくわしい歴史については、よほど初期から力を入れて追いかけてい
ないとご存じない情報でしょう。
```

- わかる。この情報が一番ありがたい。

---

```javascript
「愚者は
経験に学び、賢者は歴史に学ぶ」とはドイツ初代宰相のビスマルクの言葉ですが、てっとりばやく使い方
だけを身につけ、実践で失敗を重ねながら開発していけば、そのうち正解に行き着くことはできるでし
ょう。しかしそこに至る道のりは長く、その後には無数の技術的負債が残ります。

```

- 金言

---

```javascript
「パッケージのインストールと整合性の管理の問題ですか……。それって Ruby の gem8 や Bundler
9 がや
ってくれてるようなものですよね」
「そうだね。JavaScript の世界でそれを担ってるのが npm10 なの。
```

```javascript
「End to End」テストの意。「端から端まで」の言葉通り、Web アプリケーションにおいては実際のユーザー
が行うようにブラウザを操作して、期待通りの挙動となるかをシナリオに沿って確認するテストのこと。
```

```
npm list -g --depth=0
```

https://diwao.com/2019/09/npm-list-global.html

```javascript
「へー、npm と比べて Yarn は何が優れていたんですか？」
「たとえば依存パッケージのバージョン固定ができる yarn.lock によるロックファイルのしくみが最初
から備わっていた。これは npm がバージョン 5.0.0 で追随して同じことができるようになった。また複数
のプロジェクトを単一のリポジトリで管理するための Workspace という機能も Yarn が先行して実装し、
後年になって npm が追随した。こんなふうに Yarn が先に実装して普及した機能を npm が後追いすると
いう構図があったの」
```

---

## JavaScriptってそうなのー？

```javascript
JavaScript では Java と同様、データ型が『プリミティブ型』と
『オブジェクト型』の 2 つに大別される
```

```javascript
null と undefined を除くすべてのプリミティブ型には、それらの値を抱合する『ラッパーオブジ
ェクト（Wrapper Object）』というものが存在してる。String 型なら String
59、Number 型なら Number
60 が
それに相当する。これらは JavaScript の言語処理系に標準組み込みオブジェクト 61 として備わってる。
```

```javascript
> function fn() {};
> fn.__proto__.constructor.name
'Function'
> fn.__proto__.__proto__.constructor.name
'Object'
「……これって関数が配列や狭義のオブジェクトとかと同じく、Object を祖先に持ったオブジェクトだ
ってことでしょうか？ 関数がオブジェクトってどういうことかピンと来ませんけど」
「そう。JavaScript では関数は組み込みオブジェクト Function
68 のインスタンスであり、『第一級オブジェ
クト（First-Class Object）』でもあるの」
```

```javascript
// Function インスタンスの生成
const sum = new Function("n", "m", "return n + m;");
// 関数式（＝関数リテラル）
const add = function (n, m) {
  return n + m;
};
```

```javascript
「なお関数は第一級オブジェクトだといったけど、ゆえにオブジェクトのプロパティ値にもなれる。そし
て JavaScript においては、単にオブジェクトのプロパティとなってる関数のことを『メソッド』と呼ぶの」
リスト 12: 04-function/method-def.js
const foo = {
bar: 'bar',
baz: function () {
console.log('I am `baz` method');
},
};
foo.baz(); // I am `baz` method
```

```javascript
「そう。つまり関数式による関数の定義っていうのは実際のところ、無名関数をいったん生成した上で
それを変数に代入することによってメモリ上に残してるってことなのね。これも単なる考え方のちがいと
いってしまえばそれまでなんだけど、このさき関数型プログラミングをやっていく上でこのことを認識し
てるのとしてないのとでは、その人が書くコードの質が全然ちがってくるから」
```

---

```javascript
> [1, 2, 3].__proto__
[]
> { foo: 'bar' }.__proto__
{}
> 'JavaScript'.__proto__
[String: '']
> (65536).__proto__
[Number: 0]

```

```javascript
> new Array(1, 2, 3);
[ 1, 2, 3 ]
> typeof Array // Array はコンストラクタ関数
'function'
> Array.prototype // Array に設定されているプロトタイプは []
[]
> new String('JavaScript');
[String: 'JavaScript']
> typeof String // String はコンストラクタ関数
'function'
> String.prototype // String に設定されているプロトタイプは ''
[String: '']

```

- 言ってることはわかる。
- 大元のprototypeがStringとかのプリミティブなそれじゃだめだったんでしょうか

```javascript
> Object.prototype.toString();
'[object Object]'
> hawk.toString();
'[object Object]'
> Bird.prototype.toString = function () { return `Bird { name: ${this.name} }`; };
> hawk.toString();
'Bird { name: タカ }'
```

```javascript
const user = {
  id: 1,
  name: "Patty Rabbit",
  email: "patty@maple.town",
  age: 8,
};
const { id, ...userWithoutId } = user;
console.log(id, userWithoutId);
// 1 { name: 'Patty Rabbit', email: 'patty@maple.town', age: 8 }
```

- えっ、これすご！

```javascript
const original = { a: 1, b: 2, c: 3 };
const copy = { ...original };
console.log(copy); // { a: 1, b: 2, c: 3 }
console.log(copy === original); // false
const assigned = { ...original, ...{ c: 10, d: 50 }, d: 100 };
console.log(assigned); // { a: 1, b: 2, c: 10, d: 100 }
console.log(original); // { a: 1, b: 2, c: 3 }
```

- object強いなー...stateの更新もだいたいこれ

```javascript
「うん。ただどっちの方法も気をつけないといけないのは、これらはシャローコピー（Shallow Copy）と
いって、コピーされるオブジェクトの深さが 1 段階までしか有効じゃないこと。プロパティの値がさらに
配列やオブジェクトだった場合に、それらの値までコピーしてくれるものではないの」

「これがシャローコピーであることを意識しなかったときに起きる弊害ね。プロパティ値が文字列や数値
はそのまま値がコピーされるけど、オブジェクトだった場合はそのオブジェクトへの参照がコピーされて
るだけ。だからネストしているオブジェクトをまるごとコピーしたいなら、すべての階層に渡って再帰的
に値をコピーしてやる必要があるわけ」
```

```javascript
「コンストラクタ関数を呼ぶときその前に置く new 演算子 105 だけど、JavaScript では実はコンストラクタ
専用ってわけじゃなくて、意味があるかどうかは別としてあらゆる関数につけて実行できる。
このときの new 演算子が何をやっているかというと、その関数の prototype オブジェクトをコピーし
て新規にオブジェクトを作り、次にそれを関数に this として渡す。最後にその関数が return this で終
わってない場合でも暗黙的に return this を実行してあげるの」
リスト 37: ※ Node.js v14.4.0 以前で実行のこと

> function dump() { console.log('`this` is', this); }
> const obj = new dump();
`this` is dump {}
> obj
dump {}
> dump.prototype
dump {}
> obj !== dump.prototype // obj は dump.prototype とアドレスを共有しない新規のオブジェクト
true
```

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    function doIt() {
      console.log(`Hi, I'm ${this.name}`);
    }
    doIt();
  }
}
const minky = new Person("Momo");
minky.greet(); // thisの参照がここになるからglobal汚染しようとするけど、use strict下であれば this = undefined
// TypeError: Cannot read property 'name' of undefined
```

- // thisの参照がここになるからglobal汚染しようとするけど、use strict下であれば this = undefined

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  greet1() {
    function doIt() {
      console.log(`1) Hi, I'm ${this.name}`);
    }
    const boundDoIt = doIt.bind(this); // 1. 関数にインスタンスの this を束縛
    boundDoIt();
  }
  greet2() {
    function doIt() {
      console.log(`2) Hi, I'm ${this.name}`);
    }
    doIt.call(this); // 2. インスタンスの this を渡して実行
  }
  greet3() {
    const _this = this; // 3. 変数 _this に値を移し替える
    function doIt() {
      console.log(`3) Hi, I'm ${_this.name}`);
    }
    doIt();
  }
  greet4() {
    const doIt = () => {
      // 4. アロー関数式で定義
      console.log(`4) Hi, I'm ${this.name}`);
    };
    doIt();
  }
  greet5 = () => {
    // 5. メソッド自身もアロー関数式で定義
    const doIt = () => {
      console.log(`5) Hi, I'm ${this.name}`);
    };
    doIt();
  };
}
const creamy = new Person("Mami");
creamy.greet1(); // 1) Hi, I'm Mami
creamy.greet2(); // 2) Hi, I'm Mami
creamy.greet3(); // 3) Hi, I'm Mami
creamy.greet4(); // 4) Hi, I'm Mami
creamy.greet5(); // 5) Hi, I'm Mami
```

```
「タグの中に type="module" という属性の指定があるでしょ。これが <script> タグの中のコードをモジ
ュールとして定義するためのものなの。これがないとコードがすべてグローバルスコープに展開される従
来のスクリプト形式で定義されてしまうため、import 文が使えない」

```
