# Introduction
# イントロダクション

## Enumerables

In Ember.js, an Enumerable is any object that contains a number of child
objects, and which allows you to work with those children using the
[Ember.Enumerable](http://emberjs.com/api/classes/Ember.Enumerable.html) API. The most common
Enumerable in the majority of apps is the native JavaScript array, which
Ember.js extends to conform to the Enumerable interface.

Ember.jsでは、Enumerableは多数の子オブジェクトを包含するあらゆるオブジェクトです。そしてそれは[Ember.Enumerable](http://emberjs.com/api/classes/Ember.Enumerable.html) APIを使うことで、それらの子を使って仕事をすることを可能にします。大多数のアプリケーションにおいて、もっとも一般的なEnumerableはネイティブのJavaScript配列です。Ember.jsはそのJavaScript配列を、Enumerableインターフェースに従うように拡張しています。

By providing a standardized interface for dealing with enumerables,
Ember.js allows you to completely change the way your underlying data is
stored without having to modify the other parts of your application that
access it.

enumerablesを扱う上での標準化されたインターフェースを提供することで、Ember.jsは、あなたの基礎データが保存される方法を、そのデータにアクセスするアプリケーションの他の部分を修正することなしに、あなたが完璧に変えることができるようにします。

For example, you might display a list of items from fixture data during
development. If you switch the underlying data from synchronous fixtures
to an array that fetches data from the server lazily, your view,
template and controller code do not change at all.

例えば、あなたが開発の間、フィクスチャデータから項目のリストを表示したいとしましょう。もしあなたが基礎データを、同期フィクスチャから、サーバーからデータをゆっくりとフェッチする配列に切り替えたとしても、あなたのView、テンプレート、そしてControllerのコードはまったく変更なしですむのです。

The Enumerable API follows ECMAScript specifications as much as
possible. This minimizes incompatibility with other libraries, and
allows Ember.js to use the native browser implementations in arrays
where available.

Enumerable APIはECMAScriptの仕様に可能な限り従うようにしています。このことは、他のライブラリとの非互換性を最小化し、Ember.jsが、利用可能なところでは、ネイティブ・ブラウザ実装の配列を使えるようにします。

For instance, all Enumerables support the standard `forEach` method:

例えば、全てのEnumerableは標準の`forEach`メソッドをサポートします。

```javascript
[1,2,3].forEach(function(item) {
  console.log(item);
});

//=> 1
//=> 2
//=> 3
```

In general, Enumerable methods, like `forEach`, take an optional second
parameter, which will become the value of `this` in the callback
function:

一般的に、`forEach`のようなEnumerableのメソッドは、オプションで二つ目のパラメーターを取ります。これはコールバック関数の中で、`this`の値になります。

```javascript
var array = [1,2,3];

array.forEach(function(item) {
  console.log(item, this.indexOf(item));
}, array)

//=> 1 0
//=> 2 1
//=> 3 2
```

### Enumerables in Ember.js
### Ember.jsにおけるEnumerables

Usually, objects that represent lists implement the Enumerable interface. Some examples:

通常、リストを表すオブジェクトはEnumerableインターフェースを実装しています。いくつかの例を挙げると：

 * **Array** - Ember extends the native JavaScript `Array` with the
   Enumerable interface (unless you [disable prototype
   extensions.](http://emberjs.com/guides/configuring-ember/disabling-prototype-extensions/))
**Array** - EmberはネイティブのJavaScriptの`Array`をEnumerableインターフェースで拡張しています（あなたが[prototype拡張を無効](http://emberjs.com/guides/configuring-ember/disabling-prototype-extensions/)にした場合を除きます）。
 * **Ember.ArrayController** - A controller that wraps an underlying array and
   adds additional functionality for the view layer.
**Ember.ArrayController** - 基礎的な配列をラップし、Viewレイヤーのために追加の機能を加えるコントローラーです。
 * **Ember.Set** - A data structure that can efficiently answer whether it
   includes an object.
**Ember.Set** - オブジェクトを含んでいるか否か、効率的に答えてくれるデータ構造です。

### API Overview
### APIオーバービュー

In this guide, we'll explore some of the most common Enumerable
conveniences. For the full list, please see the [Ember.Enumerable API
reference documentation.](http://emberjs.com/api/classes/Ember.Enumerable.html)

このガイドでは、私たちはいくつかの最も一般的なEnumerableの利便性について見て行きます。全てのリストを見るには、[Ember.Enumerable API reference documentation](http://emberjs.com/api/classes/Ember.Enumerable.html)を参照してください。

#### Iterating Over an Enumerable
#### ENUMERABLEのイテレート

To enumerate all the values of an enumerable object, use the `forEach` method:

Enumerableオブジェクトの全ての値を走査するには、`forEach`メソッドを使います。

```javascript
var food = ["Poi", "Ono", "Adobo Chicken"];

food.forEach(function(item, index) {
  console.log('Menu Item %@: %@'.fmt(index+1, item));
});

// Menu Item 1: Poi
// Menu Item 2: Ono
// Menu Item 3: Adobo Chicken
```

#### Making an Array Copy
### 配列のコピーの作成

You can make a native array copy of any object that implements
`Ember.Enumerable` by calling the `toArray()` method:

`toArray()`メソッドを呼ぶ事で、`Ember.Enumerable`を実装しているあらゆるオブジェクトのネイティブ配列のコピーをつくることができます。

```javascript
var states = Ember.Set.create();

states.add("Hawaii");
states.add("California")

states.toArray()
//=> ["Hawaii", "California"]
```

Note that in many enumerables, such as the `Ember.Set` used in this
example, the order of the resulting array is not guaranteed.

多くのenumerables（この例で使われている`Ember.Set`など）では、結果の配列の順序は保証されないことに注意してください。

#### First and Last Objects
#### 最初と最後のオブジェクト

All Enumerables expose `firstObject` and `lastObject` properties
that you can bind to.

全てのEnumerablesはバインド可能な`firstObject`プロパティと`lastObject`プロパティを持っています。

```javascript
var animals = ["rooster", "pig"];

animals.get('lastObject');
//=> "pig"

animals.pushObject("peacock");

animals.get('lastObject');
//=> "peacock"
```

#### Map

You can easily transform each item in an enumerable using the
`map()` method, which creates a new array with results of calling a
function on each item in the enumerable.

`map()`メソッドを使うことで、enumerableの各項目を簡単に変換することができます。`map()`メソッドはenumerableの各項目について関数呼び出しを行い、その結果を使った新しい配列を作ります。

```javascript
var words = ["goodbye", "cruel", "world"];

var emphaticWords = words.map(function(item) {
  return item + "!";
});
// ["goodbye!", "cruel!", "world!"]
```

If your enumerable is composed of objects, there is a `mapBy()`
method that will extract the named property from each of those objects
in turn and return a new array:

もしあなたのenumerableがオブジェクトから構成されているなら、それらの各オブジェクトの名前が付けられたプロパティを順々に抽出し、新しい配列を返す`mapBy()`というメソッドが使えます。

```javascript
var hawaii = Ember.Object.create({
  capital: "Honolulu"
});

var california = Ember.Object.create({
  capital: "Sacramento"
});

var states = [hawaii, california];

states.mapBy('capital');
//=> ["Honolulu", "Sacramento"]
```

#### Filtering
#### フィルタリング

Another common task to perform on an Enumerable is to take the
Enumerable as input, and return an Array after filtering it based on
some criteria.

Enumerableで実行する他の一般的なタスクは、Enumerableを入力として取り、ある基準値を元にしたフィルタリングを行った後の配列を返すことです。

For arbitrary filtering, use the `filter` method.  The filter method
expects the callback to return `true` if Ember should include it in the
final Array, and `false` or `undefined` if Ember should not.

任意のフィルタリングでは、`filter`メソッドを使ってください。このfilterメソッドはコールバックに、Emberが最終的な配列にそれを含めるべきなら`true`を、そうでないなら`false`か`undefined`を返すことを期待します。

```javascript
var arr = [1,2,3,4,5];

arr.filter(function(item, index, self) {
  if (item < 4) { return true; }
})

// returns [1,2,3]
```

When working with a collection of Ember objects, you will often want to filter a set of objects based upon the value of some property. The `filterBy` method provides a shortcut.

Emberオブジェクトのコレクションを操作する場合、しばしばあるプロパティの値に基づいてオブジェクトのセットをフィルタリングしたい場合があります。`filterBy`メソッドはそのショートカットを提供します。

```javascript
Todo = Ember.Object.extend({
  title: null,
  isDone: false
});

todos = [
  Todo.create({ title: 'Write code', isDone: true }),
  Todo.create({ title: 'Go to sleep' })
];

todos.filterBy('isDone', true);

// returns an Array containing only items with `isDone == true`
```

If you want to return just the first matched value, rather than an Array containing all of the matched values, you can use `find` and `findBy`, which work just like `filter` and `filterBy`, but return only one item.

もしあなたが、マッチした全ての値を持つ配列でなく、最初にマッチした値だけを返したいなら、`find`または`findBy`メソッドを使えます。これらは`filter`と`filterBy`と同様に働きますが、ただ一つの項目のみを返す点が異なります。

#### Aggregate Information (All or Any)
#### 情報の統合（すべてまたはいずれか）

If you want to find out whether every item in an Enumerable matches some condition, you can use the `every` method:

もしEnumerableの全ての項目がある条件にマッチするかどうかを調べたいなら、`every`メソッドが使えます。

```javascript
Person = Ember.Object.extend({
  name: null,
  isHappy: false
});

var people = [
  Person.create({ name: 'Yehuda', isHappy: true }),
  Person.create({ name: 'Majd', isHappy: false })
];

people.every(function(person, index, self) {
  if(person.get('isHappy')) { return true; }
});

// returns false
```

If you want to find out whether at least one item in an Enumerable matches some conditions, you can use the `some` method:

もしEnumerableのうち最低一つの項目がある条件にマッチしているかどうかを調べたいなら、`some`メソッドが使えます。

```javascript
people.some(function(person, index, self) {
  if(person.get('isHappy')) { return true; }
});

// returns true
```

Just like the filtering methods, the `every` and `some` methods have analogous `isEvery` and `isAny` methods.

ちょうどフィルタリングメソッドのように、`every`と`some`メソッドは、類似バージョンである`isEvery`メソッドと`isAny`メソッドを持っています。

```javascript
people.isEvery('isHappy', true) // false
people.isAny('isHappy', true)  // true
```

(The original document’s commit SHA1: eba88b540cd4db94820be08030eb98ae5631c1b1)