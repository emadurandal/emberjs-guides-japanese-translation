# DISABLING PROTOTYPE EXTENSIONS
# Prototype拡張の無効化


By default, Ember.js will extend the prototypes of native JavaScript
objects in the following ways:

標準では、Ember.jsはネイティブのJavaScriptオブジェクトのPrototypeを次のように拡張します。

* `Array` is extended to implement the `Ember.Enumerable`,
  `Ember.MutableEnumerable`, `Ember.MutableArray` and `Ember.Array`
  interfaces. This polyfills ECMAScript 5 array methods in browsers that
  do not implement them, adds convenience methods and properties to
  built-in arrays, and makes array mutations observable.
  `Array`は`Ember.Enumerable`、`Ember.MutableEnumerable`、`Ember.MutableArray`、`Ember.Array`インターフェースを実装するように拡張されます。これは、ECMAScript 5の配列のメソッドを（これらのメソッドをサポートしていない）ブラウザに追加し、便利なメソッドとプロパティをビルトインの配列に追加し、そして配列の変化を観察可能にします。
* `String` is extended to add convenience methods, such as
  `camelize()` and `fmt()`. 
  `String`は`camelize()`や`fmt()`のような便利なメソッドを追加するように拡張されます。
* `Function` is extended with methods to annotate functions as
  computed properties, via the `property()` method, and as observers,
  via the `observes()` or `observesBefore()` methods.
  `Function`は、`property()`メソッドを通してComputed Propertiesとして、そして`observes()`または`observesBefore()`メソッドを通してObserverとして、関数を注釈付けするメソッドで拡張されます。

This is the extent to which Ember.js enhances native prototypes. We have
carefully weighed the tradeoffs involved with changing these prototypes,
and recommend that most Ember.js developers use them. These extensions
significantly reduce the amount of boilerplate code that must be typed.

これが、Ember.jsがネイティブのprototypeを強化する範囲です。私たちは、これらのprototypeの変更に関わるトレードオフを注意深く熟考してきました。そして、Ember.jsを使うほとんどの開発者にこれらを使うことを勧めます。これらの拡張は、タイプしなければならない決まりきったコードの量を著しく減らします。

However, we understand that there are cases where your Ember.js
application may be embedded in an environment beyond your control. The
most common scenarios are when authoring third-party JavaScript that is
embedded directly in other pages, or when transitioning an application
piecemeal to a more modern Ember.js architecture.

しかしながら、私たちは、あなたのEmber.jsアプリケーションが、あなたにはどうしようもできないような環境に組み込まれるかもしれないケースもあることを理解しています。もっともよくあるシナリオとしては、他のページに直接埋め込まれたサードパーティーのJavaScriptを編集しなければならない場合、あるいはアプリケーションを段階的によりモダンなEmber.jsアーキテクチャに移行させる場合などです。

In those cases, where you can't or don't want to modify native
prototypes, Ember.js allows you to completely disable the extensions
described above.

そのような、ネイティブのprototypeを修正できない、またはしたくないケースにおいて、Ember.jsでは前述の拡張を完全に無効にすることができます。

To do so, simply set the `EXTEND_PROTOTYPES` flag to `false`:

そうするためには、単に`EXTEND_PROTOTYPES`フラグを`false`にセットします。

```javascript
window.ENV = {};
ENV.EXTEND_PROTOTYPES = false;
```

Note that the above code must be evaluated **before** Ember.js loads. If
you set the flag after the Ember.js JavaScript file has been evaluated,
the native prototypes will already have been modified.

上記のコードはEmber.jsがロードされる **前に** 評価されなければならないことに注意してください。Ember.js JavaScriptファイルが評価されたあとで、あなたがこのフラグをセットしても、すでにネイティブのprototypeは修正されてしまっています。

### Life Without Prototype Extension
### Prototype拡張なしでのコーディング

In order for your application to behave correctly, you will need to
manually extend or create the objects that the native objects were
creating before.

あなたのアプリケーションが正常に動作するために、あなたは、ネイティブオブジェクトが事前に作成していたオブジェクトを、手動で拡張するか作る必要があるでしょう。

#### Arrays
#### 配列

Native arrays will no longer implement the functionality needed to
observe them. If you disable prototype extension and attempt to use
native arrays with things like a template's `{{#each}}` helper, Ember.js
will have no way to detect changes to the array and the template will
not update as the underlying array changes.

ネイティブの配列は、もはや配列を見張るのに必要な機能を実装しません。もしprototype拡張を無効にした上で、テンプレートの`{{#each}}`ヘルパーのようなものにネイティブ配列を使おうとするのなら、Ember.jsはもはや配列の変化を検知する方法を持たず、テンプレートは配列の変化に応じた更新を行いません。

Additionally, if you try to set the model of an
`Ember.ArrayController` to a plain native array, it will raise an
exception since it no longer implements the `Ember.Array` interface.

加えて、もしあなたが　`Ember.ArrayController`のモデルにネイティブ配列をセットしようとするなら、ネイティブ配列が`Ember.Array`インターフェースを実装していないために、例外が投げられます。

You can manually coerce a native array into an array that implements the
required interfaces using the convenience method `Ember.A`:

あなたは、便利な`Ember.A`メソッドを使うことで、ネイティブ配列を、要求されたインターフェースを実装した配列に手動で変化させることができます。

```javascript
var islands = ['Oahu', 'Kauai'];
islands.contains('Oahu');
//=> TypeError: Object Oahu,Kauai has no method 'contains'

// Convert `islands` to an array that implements the
// Ember enumerable and array interfaces
Ember.A(islands);

islands.contains('Oahu');
//=> true
```

#### Strings

Strings will no longer have the convenience methods described in the
[Ember.String API reference.](http://emberjs.com/api/classes/Ember.String.html). Instead,
you can use the similarly-named methods of the `Ember.String` object and
pass the string to use as the first parameter:

文字列は、もはや[Ember.String API reference](http://emberjs.com/api/classes/Ember.String.html)に記載されている便利なメソッドを備えていません。その代わり、`Ember.String`オブジェクトで似た名前のメソッドを使うことができ、文字列を最初のパラメーターとしてそれに渡すことで使用できます。

```javascript
"my_cool_class".camelize();
//=> TypeError: Object my_cool_class has no method 'camelize'

Ember.String.camelize("my_cool_class");
//=> "myCoolClass"
```

#### Functions

To annotate computed properties, use the `Ember.computed()` method to
wrap the function:

Computed Propetiesの機能を追加するためには、関数を`Ember.computed`メソッドで囲んでください。

```javascript
// This won't work:
fullName: function() {
  return this.get('firstName') + ' ' + this.get('lastName');
}.property('firstName', 'lastName')


// Instead, do this:
fullName: Ember.computed('firstName', 'lastName', function() {
  return this.get('firstName') + ' ' + this.get('lastName');
})
```

Observers are annotated using `Ember.observer()`:

`Ember.observer()`を使うことで、Observerの機能を追加することができます。

```javascript
// This won't work:
fullNameDidChange: function() {
  console.log("Full name changed");
}.observes('fullName')


// Instead, do this:
fullNameDidChange: Ember.observer('fullName', function() {
  console.log("Full name changed");
})
```


(The original document’s commit SHA1: 17d4c6b66bf9cfb66761f59b3f168009ddcf2fcc)