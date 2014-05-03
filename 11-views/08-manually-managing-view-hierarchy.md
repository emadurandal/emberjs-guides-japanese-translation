# MANUALLY MANAGING VIEW HIERARCHY
# 手動によるView階層の管理

## Ember.ContainerView

As you probably know by now, views usually create their child views
by using the `{{view}}` helper. However, it is sometimes useful to
_manually_ manage a view's child views.
[`Ember.ContainerView`](http://emberjs.com/api/classes/Ember.ContainerView.html)
is the way to do just that.

今やあなたは知っているかもしれませんが、Viewは通常、`{{view}}`ヘルパーを使うことで、子Viewを生成します。しかし、Viewの子Viewを_手動で_管理することは、ときには有用です。[`Ember.ContainerView`](http://emberjs.com/api/classes/Ember.ContainerView.html)は、まさにそれをおこなう方法です。

As you programmatically add or remove views to a `ContainerView`,
those views' rendered HTML are added or removed from the DOM to
match.

あなたがプログラム的に`ContainerView`にViewを追加したり取り除いたりすることで、それらのViewのレンダリングされたHTMLはDOMに追加されたり、削除されたりします。

```javascript
var container = Ember.ContainerView.create();
container.append();

var firstView = App.FirstView.create(),
    secondView = App.SecondView.create();

container.pushObject(firstView);
container.pushObject(secondView);

// When the rendering completes, the DOM
// will contain a `div` for the ContainerView
// and nested inside of it, a `div` for each of
// firstView and secondView.
```

### Defining the Initial Views of a Container View
### Container Viewの初期Viewの定義

There are a few ways to specify which initial child views a
`ContainerView` should render. The most straight-forward way is to add
them in `init`:

`ContainerView`がレンダリングすべき初期子Viewを指定するいくつかの方法があります。最も直接的な方法は、`init`メソッドでそれらを加えることです。

```javascript
var container = Ember.ContainerView.create({
  init: function() {
    this._super();
    this.pushObject(App.FirstView.create());
    this.pushObject(App.SecondView.create());
  }
});

container.objectAt(0).toString(); //=> '<App.FirstView:ember123>'
container.objectAt(1).toString(); //=> '<App.SecondView:ember124>'
```

As a shorthand, you can specify a `childViews` property that will be
consulted on instantiation of the `ContainerView` also. This example is
equivalent to the one above:

より短い書き方として、`ContainerView`のインスタンス化について相談を受けるプロパティである`childViews`を指定することもできます。この例は上のコードと同等です。

```javascript
var container = Ember.ContainerView.extend({
  childViews: [App.FirstView, App.SecondView]
});

container.objectAt(0).toString(); //=> '<App.FirstView:ember123>'
container.objectAt(1).toString(); //=> '<App.SecondView:ember124>'
```

Another bit of syntactic sugar is available as an option as well:
specifying string names in the `childViews` property that correspond
to properties on the `ContainerView`. This style is less intuitive
at first but has the added bonus that each named property will
be updated to reference its instantiated child view:

その他のちょっとしたシンタックスシュガーもオプションとして利用可能です。すなわち、`ContainerView`のプロパティに対応した`childViews`プロパティの中にある文字列の名前を指定します。このスタイルは最初は直感的ではありませんが、それぞれの名付けられたプロパティが、そのインスタンス化された子Viewを参照するために更新されるという、利点を持っています。

```javascript
var container = Ember.ContainerView.create({
  childViews: ['firstView', 'secondView'],
  firstView: App.FirstView,
  secondView: App.SecondView
});

container.objectAt(0).toString(); //=> '<App.FirstView:ember123>'
container.objectAt(1).toString(); //=> '<App.SecondView:ember124>'

container.get('firstView').toString(); //=> '<App.FirstView:ember123>'
container.get('secondView').toString(); //=> '<App.SecondView:ember124>'
```

### It Feels Like an Array Because it _is_ an Array
### それは配列の一種だから、配列のように感じる

You may have noticed that some of these examples use `pushObject` to add
a child view, just like you would interact with an Ember array.
[`Ember.ContainerView`](http://emberjs.com/api/classes/Ember.ContainerView.html)
gains its collection-like behavior by mixing in
[`Ember.MutableArray`](http://emberjs.com/api/classes/Ember.MutableArray.html). That means
that you can manipulate the collection of views very expressively, using
methods like `pushObject`, `popObject`, `shiftObject`, `unshiftObject`, `insertAt`,
`removeAt`, or any other method you would use to interact with an Ember array.

これらの例のいくつかは、ちょうどあなたがEmberの配列を使うような感じで、子Viewを追加するために`pushObject`を使っているということに、あなたは気が付かれたでしょうか？　[`Ember.ContainerView`](http://emberjs.com/api/classes/Ember.ContainerView.html)は、[`Ember.MutableArray`](http://emberjs.com/api/classes/Ember.MutableArray.html)をミックスインすることで、そのコレクション風の動作を獲得しているのです。このことは、`pushObject`、`popObject`、`shiftObject`、`unshiftObject`、`insertAt`、`removeAt`、またはあなたがEmber配列を操作するために使うであろう他のメソッドを使うことで、あなたがViewのコレクションを非常に表情豊かに（訳者注：expressivelyの訳これでいいのか自信がありません）操作できることを意味しています。

(The original document’s commit SHA1: dba96fb4e9fa878e6b31ebddd5921ccffc236133)