# REPRESENTING MULTIPLE MODELS WITH ARRAYCONTROLLER
# ArrayControllerによる複数モデルの表現

You can use `Ember.ArrayController` to represent an array of models. To tell an
`ArrayController` which model to represent, set its `model` property
in your route's `setupController` method.

Modelの配列を表現するために、あなたは`Ember.ArrayController`を使うことができます。どのModelを表現するか`ArrayController`に伝えるために、Routeの`setupController`メソッドの中で`ArrayController`の`model`プロパティをセットします。

You can treat an `ArrayController` just like its underlying array. For
example, imagine we want to display the current playlist. In our router,
we setup our `SongsController` to represent the songs in the playlist:

あなたは`ArrayController`を、その内在的な配列のような感じで扱うことができます。例えば、現在のプレイリストを表示したいと仮定しましょう。Routerでは、プレイリストの曲を表現するために`SongsController`をセットアップします。

```javascript
App.SongsRoute = Ember.Route.extend({
  setupController: function(controller, playlist) {
    controller.set('model', playlist.get('songs'));
  }
});
```

In the `songs` template, we can use the `{{#each}}` helper to display
each song:

`songs`テンプレートでは、各曲を表示するための`{{#each}}`ヘルパーを使うことができます。

```handlebars
<h1>Playlist</h1>

<ul>
  {{#each}}
    <li>{{name}} by {{artist}}</li>
  {{/each}}
</ul>
```

You can use the `ArrayController` to collect aggregate information about
the models it represents. For example, imagine we want to display the
number of songs that are over 30 seconds long. We can add a new computed
property called `longSongCount` to the controller:

（コントローラーが表現した）Modelについての総計の情報を集めるために`ArrayController`を使うことができます。例えば、３０秒以上の長さの曲の数を表示したいと仮定しましょう。私たちは、Controllerに`longSongCount`という新しいComputed Propertyを加えることができます。

```javascript
App.SongsController = Ember.ArrayController.extend({
  longSongCount: function() {
    var longSongs = this.filter(function(song) {
      return song.get('duration') > 30;
    });
    return longSongs.get('length');
  }.property('@each.duration')
});
```

Now we can use this property in our template:

これで、私たちはこのプロパティをテンプレートで使うことができます。

```handlebars
<ul>
  {{#each}}
    <li>{{name}} by {{artist}}</li>
  {{/each}}
</ul>

{{longSongCount}} songs over 30 seconds.
```

### Sorting
### ソート

The `Ember.ArrayController` uses the `Ember.SortableMixin` to allow sorting
of content. There are two properties that can be set in order to set up sorting:

`Ember.ArrayController`はコンテンツをソートできるようにするために、`Ember.SortableMixin`を使っています。ソーティングを設定するための２つのプロパティがあります。

```javascript
App.SongsController = Ember.ArrayController.extend({
  sortProperties: ['name', 'artist'],
  sortAscending: true // false for descending
});
```

(The original document’s commit SHA1: d3201a2f1737ef2d15999f79f2b4c2568b20f677)