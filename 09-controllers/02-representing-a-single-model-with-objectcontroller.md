# ObjectControllerによる単体モデルの表現

Use `Ember.ObjectController` to represent a single model. To tell an
`ObjectController` which model to represent, set its `model`
property in your route's `setupController` method.

単体モデルを表現するのには`Ember.ObjectController`を使います。表現すべきモデルを`ObjectController`に伝えるには、Routeの`setupController`メソッドの中で`Ember.ObjectController`の`model`プロパティをセットします。

When a template asks an `ObjectController` for a property, it will first
check to see if it has its own property with that name defined. If so, it will
return its current value.

テンプレートが`ObjectController`にプロパティを問い合わせるとき、`ObjectController`は最初に、自分が、定義された名前の自身のプロパティを持っているか確認します。もしそうなら、`ObjectController`の現在のプロパティの値を返します。

However, if the controller does not have a property with that name defined, it
will return the value of the property on the model.

しかし、もしControllerが定義された名前のプロパティを持っていなかったら、Modelにあるプロパティの値を返します。

For example, imagine you are writing a music player. You have defined
your `SongController` to represent the currently playing song.

例えば、あなたがミュージックプレーヤーアプリケーションを書いていると想像してください。あなたは現在再生している曲を表現するために、`SongController`を定義しました。

```javascript
App.SongController = Ember.ObjectController.extend({
  soundVolume: 1
});
```

In your router, you set the `model` of the controller to the
currently playing song:

Routerでは、現在再生している曲をコントローラーの`model`にセットしました。

```javascript
App.SongRoute = Ember.Route.extend({
  setupController: function(controller, song) {
    controller.set('model', song);
  }
});
```

In your template, you want to display the name of the currently playing
song, as well as the volume at which it is playing.

テンプレートでは、あなたは現在再生している曲の名前と、さらに再生中の曲のボリュームを表示したいとします。

```handlebars
<p>
  <strong>Song</strong>: {{name}} by {{artist}}
</p>
<p>
  <strong>Current Volume</strong>: {{soundVolume}}
</p>
```

Because `name` and `artist` are persisted information, and thus stored
on the model, the controller looks them up there and provides them to
the template.

`name`と`artist`は永続的な情報のため、これらはModelに保存されます。ControllerはModelからこれらを取り出して、テンプレートにこれらを供給します。

`soundVolume`, however, is specific to the current user's session, and
thus stored on the controller. The controller can return its own value
without consulting the model.

しかし、`soundVolume`は現在のユーザーのセッションに固有のものです。それゆえ、Controllerに保存します。ControllerはModelに相談することなく、自身の値を返すことができます。

The advantage of this architecture is that it is easy to get started
by accessing the properties of the model via the object controller. If,
however, you need to transform a model property for a template, there is
a well-defined place to do so without adding view-specific concerns to
the model.

このアーキテクチャの優位な点は、ObjectControllerを介してModelのプロパティにアクセスすることから始めることが容易であることです。しかしもし、テンプレートのためにmodelプロパティを変換する必要があるなら、表示固有の問題をモデルに設定するのではなく、そうするためにはっきりと決められた場所（訳者補足：Controllerのこと）があるのです。

For example, imagine we want to display the duration of the song:

例えば、曲の時間を表示したいとしましょう。

```handlebars
<p>
  <strong>Song</strong>: {{name}} by {{artist}}
</p>
<p>
  <strong>Duration</strong>: {{duration}}
</p>
```

This is saved on the server as an integer representing the number of
seconds, so our first attempt looks like this:

この値は、秒数を表す整数としてサーバーに保存されます。なので、私たちの最初の試みは次のような感じにみえるでしょう。

```html
<p>
  <strong>Song</strong>: 4 Minute Warning by Radiohead
</p>
<p>
  <strong>Duration</strong>: 257
</p>
```

Since our users are humans and not robots, however, we'd like to display
the duration as a formatted string.

しかし、私たちのユーザーは人間でありロボットではありません。私たちは時間をフォーマットされた文字列として表示したいのです。

This is very easy to do by defining a computed property on the
controller which transforms the model's value into a human-readable
format for the template:

このことは、Controllerにて、テンプレートのためにModelの値を人間が読めるフォーマットに変換するComputed Propertyを定義することで、簡単に実現することができます。

```javascript
App.SongController = Ember.ObjectController.extend({
  duration: function() {
    var duration = this.get('model.duration'),
         minutes = Math.floor(duration / 60),
         seconds = duration % 60;

    return [minutes, seconds].join(':');
  }.property('model.duration')
});
```

Now, the output of our template is a lot friendlier:

これで、テンプレートの出力はよりとてもフレンドリーになりました。

```html
<p>
  <strong>Song</strong>: 4 Minute Warning by Radiohead
</p>
<p>
  <strong>Duration</strong>: 4:17
</p>
```

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)