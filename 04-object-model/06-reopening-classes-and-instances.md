# Reopening Classes and Instances
# クラスとインスタンスのリオープン

You don't need to define a class all at once. You can reopen a class and
define new properties using the `reopen` method.

クラスの全てを一度に定義する必要はありません。`reopen`メソッドを使うことで、クラスをリオープンして、新しいプロパティを定義することができます

```javascript
Person.reopen({
  isPerson: true
});

Person.create().get('isPerson') // true
```

When using `reopen`, you can also override existing methods and
call `this._super`.

`reopen`を使うとき、既存のメソッドをオーバーライドし、また`this._super`を呼ぶこともできます。

```javascript
Person.reopen({
  // override `say` to add an ! at the end
  say: function(thing) {
    this._super(thing + "!");
  }
});
```

`reopen` is used to add instance methods and properties that are shared across all instances of a class. It does not add
methods and properties to a particular instance of a class as in vanilla JavaScript (without using prototype).

`reopen`はクラスの全てのインスタンスに渡って共有される、インスタンスメソッドとインスタンスプロパティを追加するのに使われます。reopenは、（prototypeの使用を除いて）普通のJavaScriptとして、クラスの特定のインスタンスに対してメソッドとプロパティを追加するわけではありません。

But when you need to create class methods or add properties to the class itself you can use `reopenClass`.

しかし、クラスそのものに対して、クラスメソッドを作る、またはプロパティを追加する、といった必要があるときは、`reopenClass`メソッドを使うことができます。

```javascript
Person.reopenClass({
  createMan: function() {
    return Person.create({isMan: true})
  }
});

Person.createMan().get('isMan') // true
```

(The original document’s commit SHA1: f7a35e98d357d50391cce5b8f5c9be617e929710)