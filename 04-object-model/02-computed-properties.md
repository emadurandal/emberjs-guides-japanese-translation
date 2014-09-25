## What are Computed Properties?
## Computed Propertiesとは何か？

In a nutshell, computed properties let you declare functions as properties. You create one by defining a computed property as a function, which Ember will automatically call when you ask for the property. You can then use it the same way you would any normal, static property.

簡単に説明すると、Computed Propertiesは、プロパティのように宣言する関数です。あなたが関数としてComputed Propertyを定義することでそれを１つ作ると、Emberはあなたがそのプロパティについて問い合わせた時に自動的にそれを呼び出すことになります。あなたは普通の静的なプロパティを使うのと同じ方法で、Computed Propetyを使うことができます。

It's super handy for taking one or more normal properties and transforming or manipulating their data to create a new value. 

Computed Propetiesは、一つまたはそれ以上の普通のプロパティを取得して、それらのデータを変換または操作して新しい値を作るための、非常に便利な方法です。

### Computed properties in action
### Computed Propertiesの実際

We'll start with a simple example:

簡単な例から始めましょう。

```javascript
App.Person = Ember.Object.extend({
  // these will be supplied by `create`
  firstName: null,
  lastName: null,

  fullName: function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});

var ironMan = App.Person.create({
  firstName: "Tony",
  lastName:  "Stark"
});

ironMan.get('fullName'); // "Tony Stark"
```
Notice that the `fullName` function calls `property`. This declares the function to be a computed property, and the arguments tell Ember that it depends on the `firstName` and `lastName` attributes.

`fullName`関数が`property`を呼んでいることに注意してください。こうすることで、この関数がConputed Propertyであることを宣言しているのです。そして、`property`への引数が、Emberに、このComputed Propertyが`firstName`と`lastName`アトリビュートに依存していることを伝えます。

Whenever you access the `fullName` property, this function gets called, and it returns the value of the function, which simply calls `firstName` + `lastName`.

あなたが`fullName`プロパティにアクセスするときはいつでも、この関数が呼ばれ、そして関数の値（つまり、単純な`firstName` + `lastName`の呼び出し）を返します。

#### Alternate invocation
#### 代わりの実現方法

At this point, you might be wondering how you are able to call the `.property` function on a function.  This is possible because Ember extends the `function` prototype.  More information about extending native prototypes is available in the [disabling prototype extensions guide](http://emberjs.com/guides/configuring-ember/disabling-prototype-extensions/). If you'd like to replicate the declaration from above without using these extensions you could do so with the following:

今のところ、あなたは関数に対して`.property`関数を呼ぶことがどうして可能なのか不思議に思うかもしれません。これは、Emberが`function`プロトタイプを拡張しているから可能なのです。ネイティブPrototypeの拡張についてのより詳しい情報は[Prototype拡張の無効化ガイド](http://emberjs.com/guides/configuring-ember/disabling-prototype-extensions/)を参照してください。もし、これらの拡張を使わずに、この宣言を行いたい場合は、次のようにします。

```javascript
  fullName: Ember.computed('firstName', 'lastName', function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  })
```

### Chaining computed properties
### Computed Propertiesのチェイン

You can use computed properties as values to create new computed properties. Let's add a `description` computed property to the previous example, and use the existing `fullName` property and add in some other properties:

新しいComputed Propertiesを作るための値としてComputed Propertiesを使うことが可能です。前回の例に、`description` Computed Propertyを追加してみましょう。そして、既存の`fullName`プロパティを使い、またいくつかの他のプロパティを追加します。

```javascript
App.Person = Ember.Object.extend({
  firstName: null,
  lastName: null,
  age: null,
  country: null,

  fullName: function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName'),

  description: function() {
    return this.get('fullName') + '; Age: ' + this.get('age') + '; Country: ' + this.get('country');
  }.property('fullName', 'age', 'country')
});

var captainAmerica = App.Person.create({
  firstName: 'Steve',
  lastName: 'Rogers',
  age: 80,
  country: 'USA'
});

captainAmerica.get('description'); // "Steve Rogers; Age: 80; Country: USA"
```

### Dynamic updating
### 動的な更新

Computed properties, by default, observe any changes made to the properties they depend on and are dynamically updated when they're called. Let's use computed properties to dynamically update. 

Computed Propertiesは、標準では、それらが依存しているプロパティに対してなされるあらゆる変更を監視しており、自身が呼び出されたときに動的に更新されます。

```javascript
captainAmerica.set('firstName', 'William');

captainAmerica.get('description'); // "William Rogers; Age: 80; Country: USA"
```

So this change to `firstName` was observed by `fullName` computed property, which was itself observed by the `description` property.

なので、この`firstName`に対する変更は`fullName` Computed Propertyによって監視されています。また、`fullName`自身も`description`プロパティによって監視されています。

Setting any dependent property will propagate changes through any computed properties that depend on them, all the way down the chain of computed properties you've created.

どの依存プロパティに対する値のセットも、それらに依存しているComputed Propertiesを通して、その変更が伝播されます。あなたが作成したComputed Propertiesのチェインの端から端までです。

### Setting Computed Properties
### Computed Propertiesへの値のセット

You can also define what Ember should do when setting a computed property. If you try to set a computed property, it will be invoked with the key (property name), the value you want to set it to, and the previous value.

あなたは、Computed Propertyに対して値をセットするとき、Emberが何をすべきかを定義することもできます。あなたがComputed Propertyに対して値をセットしようとしたとき、Computed Propertyはkey（プロパティ名）とあなたがセットしたい値、そして前回の値を引数として伴って呼び出されます。

```javascript
App.Person = Ember.Object.extend({
  firstName: null,
  lastName: null,

  fullName: function(key, value, previousValue) {
    // setter
    if (arguments.length > 1) {
      var nameParts = value.split(/\s+/);
      this.set('firstName', nameParts[0]);
      this.set('lastName',  nameParts[1]);
    }

    // getter
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});


var captainAmerica = App.Person.create();
captainAmerica.set('fullName', "William Burnside");
captainAmerica.get('firstName'); // William
captainAmerica.get('lastName'); // Burnside
```

Ember will call the computed property for both setters and getters, so if you want to use a computed property as a setter, you'll need to check the number of arguments to determine whether it is being called as a getter or a setter. Note that if a value is returned from the setter, it will be cached as the property’s value.

Emberはsetterとgetter両方のためにComputed Propertyを呼び出します。なので、もしあなたがsetterとしてComputed Propertyを使いたい場合は、getterとして呼び出されたのかsetterとして呼び出されたのかを判断するために引数の数をチェックする必要があります。もし、setterから値がreturnされた場合、その値はプロパティの値としてキャッシュされることに注意してください。

(The original document’s commit SHA1: 5f00e55cd9d20b061e90dfc91a712651e5f47da6)