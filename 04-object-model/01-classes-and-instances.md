# CLASSES AND INSTANCES
# クラスとインスタンス

To define a new Ember _class_, call the `extend()` method on
`Ember.Object`:

新しいEmberの_クラス_を定義するためには、`Ember.Object`の`extend()`メソッドを呼びます。

```javascript
App.Person = Ember.Object.extend({
  say: function(thing) {
    alert(thing);
  }
});
```

This defines a new `App.Person` class with a `say()` method.

このコードは、`say()`メソッドを持った新しい`App.Person`クラスを定義します。

You can also create a _subclass_ from any existing class by calling
its `extend()` method. For example, you might want to create a subclass
of Ember's built-in `Ember.View` class:

`extend()`メソッドを呼ぶことで、どのような既存のクラスからも_サブクラス_を作ることができます。例えば、あなたは、Emberにビルトインされている`Ember.View`クラスのサブクラスを作りたいと思うかもしれません。

```js
App.PersonView = Ember.View.extend({
  tagName: 'li',
  classNameBindings: ['isAdministrator']
});
```

When defining a subclass, you can override methods but still access the
implementation of your parent class by calling the special `_super()`
method:

サブクラスを定義するとき、あなたはメソッドをオーバーライドできますが、特別な`_super()`メソッドを呼ぶことで、親クラスの実装にアクセスする事ができます。

```javascript
App.Person = Ember.Object.extend({
  say: function(thing) {
    var name = this.get('name');
    alert(name + " says: " + thing);
  }
});

App.Soldier = App.Person.extend({
  say: function(thing) {
    this._super(thing + ", sir!");
  }
});

var yehuda = App.Soldier.create({
  name: "Yehuda Katz"
});

yehuda.say("Yes"); // alerts "Yehuda Katz says: Yes, sir!"
```

### Creating Instances
### インスタンスの生成

Once you have defined a class, you can create new _instances_ of that
class by calling its `create()` method. Any methods, properties and
computed properties you defined on the class will be available to
instances:

一度クラスを定義したら、そのクラスの`create()`メソッドを呼ぶことで、クラスの新しい_インスタンス_を作ることができます。あなたがそのクラスで定義したあらゆるメソッド、プロパティ、Computed Propertiesがそのインスタンスで利用可能です。

```javascript
var person = App.Person.create();
person.say("Hello"); // alerts " says: Hello"
```

When creating an instance, you can initialize the value of its properties
by passing an optional hash to the `create()` method:

インスタンスを生成するとき、`create()`メソッドにオプションのハッシュを渡す事で、そのインスタンスのプロパティの値を初期化することができます。

```javascript
App.Person = Ember.Object.extend({
  helloWorld: function() {
    alert("Hi, my name is " + this.get('name'));
  }
});

var tom = App.Person.create({
  name: "Tom Dale"
});

tom.helloWorld(); // alerts "Hi, my name is Tom Dale"
```

For performance reasons, note that you cannot redefine an instance's
computed properties or methods when calling `create()`, nor can you
define new ones. You should only set simple properties when calling
`create()`. If you need to define or redefine methods or computed
properties, create a new subclass and instantiate that.

パフォーマンス上の理由から、`create()`メソッドを呼び出すときに、インスタンスのConputed Propertiesまたはメソッドを再定義、または新しいそれらを定義することはできないことに注意してください。`create()`メソッドを呼び出すとき、単純なプロパティをセットするだけにすべきです。もしメソッドやConputed Propertiesを定義または再定義する必要があるなら、新しいサブクラスを作成し、それをインスタンス化してください。

By convention, properties or variables that hold classes are
PascalCased, while instances are not. So, for example, the variable
`App.Person` would point to a class, while `person` would point to an instance
(usually of the `App.Person` class). You should stick to these naming
conventions in your Ember applications.

慣例により、クラスを保持しているプロパティや変数の名前は大文字から始めます。インスタンスの名前は小文字からはじめます。なので、例えば、変数`App.Person`はクラスを指しており、`person`は（通常は`App.Person`クラスの）インスタンスを指しています。あなたはEmberアプリケーションにおいて、これらの命名規則に従うべきです。

### Initializing Instances
### インスタンスの初期化

When a new instance is created, its `init` method is invoked
automatically. This is the ideal place to do setup required on new
instances:

新しいインスタンスが生成されるとき、`init`メソッドが自動的に呼び出されます。この`init`メソッドは新しいインスタンスに要求されるセットアップをおこなうのに理想的な場所です。

```js
App.Person = Ember.Object.extend({
  init: function() {
    var name = this.get('name');
    alert(name + ", reporting for duty!");
  }
});

App.Person.create({
  name: "Stefan Penner"
});

// alerts "Stefan Penner, reporting for duty!"
```

If you are subclassing a framework class, like `Ember.View` or
`Ember.ArrayController`, and you override the `init` method, make sure
you call `this._super()`! If you don't, the system may not have an
opportunity to do important setup work, and you'll see strange behavior
in your application.

もし`Ember.View`や`Ember.ArrayController`などの、フレームワークのクラスのサブクラスを定義するなら、そしてそれらの`init`メソッドをオーバーライドするなら、`this._super()`メソッドを呼ぶことを忘れないでください！　もしそれをしないと、システムは重要なセットアップ作業を行う機会を失い、あなたはアプリケーションでおかしな挙動を見ることになるでしょう。

When accessing the properties of an object, use the `get`
and `set` accessor methods:

オブジェクトのプロパティにアクセスするときは、`get`と`set`のアクセサメソッドを使います。

```js
var person = App.Person.create();

var name = person.get('name');
person.set('name', "Tobias Fünke");
```

Make sure to use these accessor methods; otherwise, computed properties won't
recalculate, observers won't fire, and templates won't update.

これらのアクセサメソッドを確実に使ってください。さもないと、Computed Propertiesは再計算されず、Observerは発火されず、テンプレートは更新されません。

(The original document’s commit SHA1: 25c8c4a7fc508187f422547c5c172bd63e937ef9)