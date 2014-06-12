## Observers

Ember supports observing any property, including computed properties.
You can set up an observer on an object by using the `observes`
method on a function:

EmberはComputed Propertiesを含めたあらゆるプロパティの監視をサポートしています。関数の`observes`メソッドを使うことで、オブジェクトのObserverをセットアップすることができます。

```javascript
Person = Ember.Object.extend({
  // these will be supplied by `create`
  firstName: null,
  lastName: null,
  
  fullName: function() {
    var firstName = this.get('firstName');
    var lastName = this.get('lastName');

    return firstName + ' ' + lastName;
  }.property('firstName', 'lastName'),

  fullNameChanged: function() {
    // deal with the change
  }.observes('fullName').on('init')
});

var person = Person.create({
  firstName: 'Yehuda',
  lastName: 'Katz'
});

person.set('firstName', 'Brohuda'); // observer will fire
```

Because the `fullName` computed property depends on `firstName`,
updating `firstName` will fire observers on `fullName` as well.

`fullName` Computed Propertyは`firstName`に依存しているので、`firstName`の更新は`fullName`のObserverを起動します。

### Observers and asynchrony
### Observerと非同期性

Observers in Ember are currently synchronous. This means that they will fire
as soon as one of the properties they observe changes. Because of this, it
is easy to introduce bugs where properties are not yet synchronized:

EmberにおけるObserverは現在は同期的です。これは、Observerが監視しているプロパティのひとつが変更されたらすぐに、Observerが起動されることを意味します。このことから、プロパティがまだ同期されていないバグを引き起こすことは容易です。

```javascript
Person.reopen({
  lastNameChanged: function() {
    // The observer depends on lastName and so does fullName. Because observers
    // are synchronous, when this function is called the value of fullName is
    // not updated yet so this will log the old value of fullName
    console.log(this.get('fullName'));
  }.observes('lastName')
});
```

This synchronous behaviour can also lead to observers being fired multiple
times when observing multiple properties:

この同期的な動作は、複数のプロパティを監視しているときは、Observerが複数回起動されることを意味します。

```javascript
Person.reopen({
  partOfNameChanged: function() {
    // Because both firstName and lastName were set, this observer will fire twice.
  }.observes('firstName', 'lastName')
});

person.set('firstName', 'John');
person.set('lastName', 'Smith');
```

To get around these problems, you should make use of `Ember.run.once`. This will
ensure that any processing you need to do only happens once, and happens in the
next run loop once all bindings are synchronized:

これらの問題を回避するに、`Ember.run.once`を確実に使用すべきです。これは、あなたがする必要がある任意の処理は一度だけ発生し、一度すべてのバインディングが同期化されると、次回の実行ループで起こることを保証します。

```javascript
Person.reopen({
  partOfNameChanged: function() {
    Ember.run.once(this, 'processFullName');
  }.observes('firstName', 'lastName'),

  processFullName: function() {
    // This will only fire once if you set two properties at the same time, and
    // will also happen in the next run loop once all properties are synchronized
    console.log(this.get('fullName'));
  }
});

person.set('firstName', 'John');
person.set('lastName', 'Smith');
```

### Observers and object initialization
### Observerとオブジェクトの初期化

Observers never fire until after the initialization of an object is complete.

Observerはオブジェクトの初期化が完了するまでは決して起動しません。

If you need an observer to fire as part of the initialization process, you
cannot rely on the side effect of set. Instead, specify that the observer
should also run after init by using `.on('init')`:

もし初期化プロセスの一部としてObserberを起動する必要がある場合は、あなたはオブジェクトの値のセットの副作用に依存してはいけません。その代わり、`.on('init')`をつかうことで、initの後にもObserverを走らせるように指定します。

```javascript
App.Person = Ember.Object.extend({
  init: function() {
    this.set('salutation', "Mr/Ms");
  },

  salutationDidChange: function() {
    // some side effect of salutation changing
  }.observes('salutation').on('init')
});
```

### Unconsumed Computed Properties Do Not Trigger Observers
### 使われないComputed PropertiesはObserverを起動しない

If you never `get` a computed property, its observers will not fire even if
its dependent keys change. You can think of the value changing from one unknown
value to another.

もし、あなたがComputed Propertyを`get`しないなら、たとえその依存しているキーが変更されたとしても、Observerは起動されません。その値は、ひとつの未知の値から他の値に変化していると考えることができます。

This doesn't usually affect application code because computed properties are
almost always observed at the same time as they are fetched. For example, you get
the value of a computed property, put it in DOM (or draw it with D3), and then
observe it so you can update the DOM once the property changes.

このことは通常アプリケーションコードに影響はもたらしません。なぜなら、Computed Propertiesはほとんど常に、それらが取り出されると同時に監視されるからです。例えば、あなたがComputed Propertyの値を取得して、それをDOMに入れて（またはD3で描画して）、それからそのプロパティを監視すると、そのプロパティが変更されるたびにDOMを更新することができます。

If you need to observe a computed property but aren't currently retrieving it,
just get it in your init method.

もし、Computed Propertyを監視する必要があって、しかし現在のところそのComputed Propertyを取得しないのであれば、initメソッドのなかで、そのComputed Propertyを取得してください。

### Without prototype extensions
### プロトタイプ拡張を使わない場合

You can define inline observers by using the `Ember.observer` method if you
are using Ember without prototype extensions:

もしEmberのプロトタイプ拡張を使わないなら、`Ember.observer`メソッドを使うことで、インラインのオブザーバーを定義することができます。


```javascript
Person.reopen({
  fullNameChanged: Ember.observer('fullName', function() {
    // deal with the change
  })
});
```

### Outside of class definitions
### クラス定義の外側

You can also add observers to an object outside of a class definition
using addObserver:

addObserverを使うことで、クラス定義の外側で、オブジェクトにObserverを追加することができます。

```javascript
person.addObserver('fullName', function() {
  // deal with the change
});
```

(The original document’s commit SHA1: 37b13598b552adf438df2bfc0b8dda6251c53617)