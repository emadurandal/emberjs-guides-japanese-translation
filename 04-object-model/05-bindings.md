# Binding
# バインディング

A binding creates a link between two properties such that when one changes, the
other one is updated to the new value automatically. Bindings can connect
properties on the same object, or across two different objects. Unlike most other
frameworks that include some sort of binding implementation, bindings in
Ember.js can be used with any object, not just between views and models.

バインディングは二つのプロパティの間にリンクを作成します。そのことによって、一つが変化すると、他の一つが自動的に新しい値に更新されます。バインディングは同じオブジェクトまたは異なるオブジェクトにまたがって、プロパティー間を繋ぐことができます。何らかのバインディング実装を含む他のほとんどのフレームワークと違い、Ember.jsにおけるバインディングは、ビューとモデル間だけでなく、あらゆるオブジェクトの間で使うことができます。

The easiest way to create a two-way binding is to use a computed alias, that
specifies the path to another object.

双方向バインディングを作成する最も簡単な方法は、他のオブジェクトへのパスを指定するComputed Aliasを使うことです。

```javascript
wife = Ember.Object.create({
  householdIncome: 80000
});

husband = Ember.Object.create({
  wife: wife,
  householdIncome: Ember.computed.alias('wife.householdIncome')
});

husband.get('householdIncome'); // 80000

// Someone gets raise.
husband.set('householdIncome', 90000);
wife.get('householdIncome'); // 90000
```

Note that bindings don't update immediately. Ember waits until all of your
application code has finished running before synchronizing changes, so you can
change a bound property as many times as you'd like without worrying about the
overhead of syncing bindings when values are transient.

バインディングはすぐには更新されないことに注意してください。Emberは、変更を同期する前に、あなたの全てのアプリケーションコードの実行が終わるまで待ちます。このことにより、値が一時的なときにバインディングの同期のオーバーヘッドについて心配することなく、あなたの好きなだけ何回でも、あなたはバインドされたプロパティを変更することができます。

## One-Way Bindings
## 一方向のバインディング

A one-way binding only propagates changes in one direction. Often, one-way
bindings are just a performance optimization and you can safely use a two-way binding
(as, of course, two-way bindings are de facto one-way bindings if you only ever change
one side). Sometimes one-way bindings are useful to achieve specific behaviour such
as a default that is the same as another property but can be overriden (e.g. a
shipping address that starts the same as a billing address but can later be changed)

一方向バインディングは一方向のみに変化が伝播します。通常、一方向バイディングはただのパフォーマンス最適化であり、あなたは双方向バインディングを安全に使うことができます（もちろん、あなたが片方だけを変更するのであれば、双方向バインディングは事実上の一方向のバインディングになります）。時には、一方向バインディングは、標準では一方のプロパティと同じ値であるが上書きされうる（例えば、配送先住所は最初は請求先住所と同じだが、後に配送先住所は請求先住所とは違う情報に変更されうる）ような、特定の動作を実現するための便利な方法となります。

```javascript
user = Ember.Object.create({
  fullName: "Kara Gates"
});

userView = Ember.View.create({
  user: user,
  userName: Ember.computed.oneWay('user.fullName')
});

// Changing the name of the user object changes
// the value on the view.
user.set('fullName', "Krang Gates");
// userView.userName will become "Krang Gates"

// ...but changes to the view don't make it back to
// the object.
userView.set('userName', "Truckasaurus Gates");
user.get('fullName'); // "Krang Gates"
```

(The original document’s commit SHA1: 384daeeacc4a17d4edaa7ce6c0936cd266e85953)